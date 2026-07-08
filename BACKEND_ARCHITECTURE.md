# Backend Architecture for AAHA Conversational Workflows

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        FRONTEND (Kiosk)                      │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  Conversational UI (One Question at a Time)            │ │
│  │  - Question Display                                    │ │
│  │  - AAHA Eyes Animation                                 │ │
│  │  - Voice Input/Output                                  │ │
│  │  - Follow-up Logic                                     │ │
│  └────────────────────────────────────────────────────────┘ │
│                             ↕                                │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  Workflow Engine (State Machine)                       │ │
│  │  - PCOS Workflow                                       │ │
│  │  - Anaemia Workflow                                    │ │
│  │  - Thyroid Workflow                                    │ │
│  │  - Mental Health Workflow                              │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                             ↕
┌─────────────────────────────────────────────────────────────┐
│                     API GATEWAY (Future)                     │
│  - Authentication                                            │
│  - Rate Limiting                                             │
│  - Request Routing                                           │
└─────────────────────────────────────────────────────────────┘
                             ↕
┌─────────────────────────────────────────────────────────────┐
│                   BACKEND SERVICES (Cloud)                   │
│  ┌──────────────────┐  ┌──────────────────┐                │
│  │ Workflow Service │  │  NLP/AI Service  │                │
│  │ - Question Flow  │  │  - Gemini API    │                │
│  │ - Logic Rules    │  │  - Sentiment     │                │
│  │ - Scoring        │  │  - Classification│                │
│  └──────────────────┘  └──────────────────┘                │
│  ┌──────────────────┐  ┌──────────────────┐                │
│  │ Report Service   │  │ Notification Svc │                │
│  │ - PDF Generation │  │  - WhatsApp      │                │
│  │ - Diet Advice    │  │  - SMS           │                │
│  │ - Recommendations│  │  - Email         │                │
│  └──────────────────┘  └──────────────────┘                │
└─────────────────────────────────────────────────────────────┘
                             ↕
┌─────────────────────────────────────────────────────────────┐
│                      DATABASE LAYER                          │
│  ┌──────────────────┐  ┌──────────────────┐                │
│  │ PostgreSQL       │  │  MongoDB         │                │
│  │ - Patient Data   │  │  - Workflow Logs │                │
│  │ - Test Results   │  │  - Conversation  │                │
│  │ - ASHA Records   │  │  - Analytics     │                │
│  └──────────────────┘  └──────────────────┘                │
└─────────────────────────────────────────────────────────────┘
```

---

## Data Models

### 1. Workflow Definition (JSON Schema)
```json
{
  "workflowId": "pcos-screening-v1",
  "version": "1.0",
  "name": "PCOS Screening Workflow",
  "phases": [
    {
      "phaseId": "opening",
      "type": "consent",
      "message": {
        "en": "From what you've said, I'd like to understand...",
        "hi": "Aapne jo bataya uske anusar..."
      },
      "voiceEnabled": true,
      "requiredConsent": true
    },
    {
      "phaseId": "symptoms",
      "type": "questionnaire",
      "questions": [
        {
          "id": "menstrual",
          "text": {
            "en": "Let's start with your periods — do they come every month, or do they skip or come late?",
            "hi": "Chaliye aapke periods se shuru karte hain..."
          },
          "type": "choice",
          "options": ["regular", "skip", "stopped"],
          "followUp": {
            "skip": {
              "text": "Roughly how often — every couple of months, or longer gaps?",
              "type": "text"
            },
            "stopped": {
              "text": "When did you last have one?",
              "type": "date"
            }
          },
          "weight": 3,
          "scoringRules": {
            "skip": 2,
            "stopped": 3,
            "regular": 0
          }
        }
      ]
    },
    {
      "phaseId": "diet",
      "type": "questionnaire",
      "questions": [...]
    },
    {
      "phaseId": "tests",
      "type": "clinical",
      "tests": [
        {
          "testId": "hemoglobin",
          "consentRequired": true,
          "consentMessage": "Now I'd like to check your haemoglobin...",
          "duration": 60,
          "optional": false
        }
      ]
    }
  ],
  "scoring": {
    "algorithm": "weighted_sum",
    "thresholds": {
      "low": 0-30,
      "moderate": 31-60,
      "high": 61-100
    }
  }
}
```

### 2. Patient Session (Frontend State)
```javascript
{
  "sessionId": "uuid",
  "kioskId": "PHC_PUNE_KIOSK_01",
  "patientId": "9876543210",
  "workflowId": "pcos-screening-v1",
  "startedAt": "2026-07-08T11:30:00Z",
  "currentPhase": "symptoms",
  "currentQuestion": 3,
  "responses": {
    "menstrual": {
      "answer": "skip",
      "followUp": {
        "frequency": "2-3 months"
      },
      "timestamp": "2026-07-08T11:32:15Z",
      "confidence": 0.95
    },
    "weight": {
      "answer": "yes",
      "followUp": {
        "duration": "1-2 years"
      },
      "timestamp": "2026-07-08T11:33:20Z"
    }
  },
  "dietInfo": {
    "type": "vegetarian",
    "meals": ["skip_breakfast"],
    "sugar": "high",
    "fried": "3-4_times_week"
  },
  "testResults": {
    "hemoglobin": {
      "value": 10.2,
      "unit": "g/dL",
      "status": "low",
      "timestamp": "2026-07-08T11:45:30Z"
    }
  },
  "score": 68,
  "riskLevel": "high",
  "completedAt": null
}
```

### 3. Backend API Endpoints

```
POST /api/v1/workflows/start
- Initialize new workflow session
- Request: {patientId, workflowId, kioskId}
- Response: {sessionId, firstQuestion, ...}

POST /api/v1/workflows/{sessionId}/answer
- Submit answer to current question
- Request: {questionId, answer, followUpAnswers}
- Response: {nextQuestion, followUpNeeded, phaseComplete}

GET /api/v1/workflows/{sessionId}/status
- Get current workflow progress
- Response: {currentPhase, progress%, estimatedTimeRemaining}

POST /api/v1/workflows/{sessionId}/test-consent
- Record test consent
- Request: {testId, consented, reason}
- Response: {status, nextAction}

POST /api/v1/workflows/{sessionId}/test-result
- Submit test result from device
- Request: {testId, value, unit, deviceId}
- Response: {stored, miniReport}

POST /api/v1/workflows/{sessionId}/complete
- Finalize workflow and generate report
- Response: {reportId, downloadUrl, score, recommendations}

GET /api/v1/reports/{reportId}
- Retrieve generated report
- Response: PDF or JSON

POST /api/v1/reports/{reportId}/share
- Share report via WhatsApp/SMS/Email
- Request: {method, recipient}
```

---

## Frontend Implementation

### State Management
```javascript
// Global workflow state
const workflowState = {
  active: false,
  sessionId: null,
  workflowType: null, // 'pcos', 'anaemia', 'thyroid', etc.
  definition: null, // Workflow JSON
  currentPhase: null,
  currentQuestionIndex: 0,
  responses: {},
  dietInfo: {},
  testResults: {},
  score: null,
  awaitingResponse: false
};

// Initialize workflow
async function startWorkflow(workflowType){
  // Load workflow definition (from JSON file or API)
  const definition = await loadWorkflowDefinition(workflowType);
  
  workflowState.active = true;
  workflowState.sessionId = generateUUID();
  workflowState.workflowType = workflowType;
  workflowState.definition = definition;
  workflowState.currentPhase = definition.phases[0];
  
  // Sync with backend (future)
  // await api.post('/api/v1/workflows/start', {...});
  
  // Start first phase
  await executePhase(workflowState.currentPhase);
}
```

### Question Engine
```javascript
async function askQuestion(question){
  const { id, text, type, options, followUp } = question;
  
  // Show AAHA eyes
  showAAHAEyesInChat();
  
  // Display question
  const displayText = text[state.language] || text.en;
  addBubble('ai', displayText);
  
  // Speak question
  const voiceText = text.hi || text.en;
  await speak(voiceText);
  
  // Mark awaiting response
  workflowState.awaitingResponse = true;
  
  // Wait for user response
  const response = await waitForUserInput();
  
  // Store response
  workflowState.responses[id] = {
    answer: response,
    timestamp: new Date().toISOString()
  };
  
  // Check if follow-up needed
  if(followUp && followUp[response]){
    await askFollowUp(id, followUp[response]);
  }
  
  // Sync with backend
  // await api.post(`/api/v1/workflows/${workflowState.sessionId}/answer`, {...});
  
  // Move to next question
  workflowState.currentQuestionIndex++;
  await askNextQuestion();
}

async function askFollowUp(parentId, followUpConfig){
  const { text, type } = followUpConfig;
  
  addBubble('ai', text);
  await speak(text);
  
  const response = await waitForUserInput();
  
  // Store follow-up
  if(!workflowState.responses[parentId].followUp){
    workflowState.responses[parentId].followUp = {};
  }
  workflowState.responses[parentId].followUp = response;
}
```

### Scoring Engine (Frontend)
```javascript
function calculateWorkflowScore(){
  const { definition, responses } = workflowState;
  let totalScore = 0;
  let maxScore = 0;
  
  // Iterate through all questions
  definition.phases.forEach(phase => {
    if(phase.type === 'questionnaire'){
      phase.questions.forEach(q => {
        const response = responses[q.id];
        if(response && q.scoringRules){
          const points = q.scoringRules[response.answer] || 0;
          totalScore += points * q.weight;
          maxScore += Math.max(...Object.values(q.scoringRules)) * q.weight;
        }
      });
    }
  });
  
  // Normalize to 0-100
  const normalizedScore = (totalScore / maxScore) * 100;
  
  // Determine risk level
  const thresholds = definition.scoring.thresholds;
  let riskLevel;
  if(normalizedScore <= thresholds.low[1]) riskLevel = 'low';
  else if(normalizedScore <= thresholds.moderate[1]) riskLevel = 'moderate';
  else riskLevel = 'high';
  
  workflowState.score = Math.round(normalizedScore);
  workflowState.riskLevel = riskLevel;
  
  return { score: workflowState.score, riskLevel };
}
```

### Diet Advice Generator
```javascript
function generateDietAdvice(){
  const { workflowType, responses, dietInfo } = workflowState;
  const advice = [];
  
  // Load condition-specific rules
  const rules = DIET_RULES[workflowType];
  
  // Analyze diet responses
  if(dietInfo.meals?.includes('skip')){
    advice.push(rules.skipMeals);
  }
  
  if(dietInfo.sugar === 'high'){
    advice.push(rules.highSugar);
  }
  
  if(dietInfo.fried === 'frequent'){
    advice.push(rules.friedFood);
  }
  
  // Add protein advice if vegetarian
  if(dietInfo.type === 'vegetarian'){
    advice.push(rules.vegetarianProtein);
  }
  
  // Add general principles
  advice.push(...rules.general);
  
  return advice;
}

const DIET_RULES = {
  pcos: {
    skipMeals: {
      en: "Don't skip meals, especially breakfast — it helps control sugar cravings and balances hormones.",
      hi: "Khana skip mat kijiye, khaas kar breakfast — yeh sugar cravings control karta hai."
    },
    highSugar: {
      en: "Reduce sugary tea, cold drinks, and sweets — PCOS makes your body sensitive to sugar spikes.",
      hi: "Meethi chai, cold drinks aur mithai kam karein — PCOS mein body sugar ke liye sensitive hoti hai."
    },
    vegetarianProtein: {
      en: "Add protein to every meal — dal, sprouts, paneer. Protein slows sugar absorption.",
      hi: "Har meal mein protein add karein — dal, ankurit anaj, paneer. Protein sugar control karta hai."
    },
    general: [
      "Fill half your plate with vegetables",
      "Choose whole grains over refined",
      "Drink 2-3 litres water daily"
    ]
  },
  anaemia: {
    // ... rules for anaemia
  }
};
```

---

## Backend Service (Node.js/Python Example)

### Workflow Service
```javascript
// Node.js Express Example
const express = require('express');
const router = express.Router();

// Start workflow
router.post('/workflows/start', async (req, res) => {
  const { patientId, workflowId, kioskId } = req.body;
  
  // Load workflow definition
  const definition = await db.workflows.findOne({ id: workflowId });
  
  // Create session
  const session = {
    sessionId: generateUUID(),
    patientId,
    kioskId,
    workflowId,
    definition,
    startedAt: new Date(),
    currentPhase: definition.phases[0].phaseId,
    responses: {},
    status: 'active'
  };
  
  await db.sessions.create(session);
  
  // Return first question
  const firstQuestion = definition.phases[0].questions[0];
  
  res.json({
    sessionId: session.sessionId,
    question: firstQuestion,
    phase: session.currentPhase
  });
});

// Submit answer
router.post('/workflows/:sessionId/answer', async (req, res) => {
  const { sessionId } = req.params;
  const { questionId, answer, followUpAnswers } = req.body;
  
  // Load session
  const session = await db.sessions.findOne({ sessionId });
  
  // Store response
  session.responses[questionId] = {
    answer,
    followUp: followUpAnswers,
    timestamp: new Date()
  };
  
  // Calculate if follow-up needed
  const question = findQuestion(session.definition, questionId);
  const needsFollowUp = question.followUp && question.followUp[answer];
  
  if(needsFollowUp){
    return res.json({
      followUpNeeded: true,
      followUpQuestion: question.followUp[answer]
    });
  }
  
  // Get next question
  const nextQuestion = getNextQuestion(session);
  
  if(!nextQuestion){
    // Phase complete
    return res.json({
      phaseComplete: true,
      nextPhase: getNextPhase(session)
    });
  }
  
  await db.sessions.update({ sessionId }, { responses: session.responses });
  
  res.json({
    nextQuestion,
    progress: calculateProgress(session)
  });
});

// Complete workflow and generate report
router.post('/workflows/:sessionId/complete', async (req, res) => {
  const { sessionId } = req.params;
  
  const session = await db.sessions.findOne({ sessionId });
  
  // Calculate score
  const score = calculateScore(session);
  
  // Generate diet advice
  const dietAdvice = generateDietAdvice(session);
  
  // Create report
  const report = {
    reportId: generateUUID(),
    sessionId,
    patientId: session.patientId,
    workflowType: session.workflowId,
    score,
    responses: session.responses,
    testResults: session.testResults,
    dietAdvice,
    recommendations: generateRecommendations(session, score),
    generatedAt: new Date()
  };
  
  await db.reports.create(report);
  
  // Generate PDF
  const pdfUrl = await generatePDF(report);
  
  res.json({
    reportId: report.reportId,
    score: report.score,
    downloadUrl: pdfUrl,
    recommendations: report.recommendations
  });
});
```

---

## Local Storage (Offline Support)

```javascript
// IndexedDB for offline workflow execution
const DB_NAME = 'AAHAWorkflows';
const DB_VERSION = 1;

async function initOfflineDB(){
  return new Promise((resolve, reject) => {
    const request = indexedDB.open(DB_NAME, DB_VERSION);
    
    request.onerror = () => reject(request.error);
    request.onsuccess = () => resolve(request.result);
    
    request.onupgradeneeded = (event) => {
      const db = event.target.result;
      
      // Workflow definitions store
      if(!db.objectStoreNames.contains('workflows')){
        db.createObjectStore('workflows', { keyPath: 'id' });
      }
      
      // Sessions store
      if(!db.objectStoreNames.contains('sessions')){
        const store = db.createObjectStore('sessions', { keyPath: 'sessionId' });
        store.createIndex('patientId', 'patientId', { unique: false });
        store.createIndex('status', 'status', { unique: false });
      }
      
      // Pending sync store
      if(!db.objectStoreNames.contains('pendingSync')){
        db.createObjectStore('pendingSync', { keyPath: 'id', autoIncrement: true });
      }
    };
  });
}

// Sync with backend when online
async function syncWithBackend(){
  if(!navigator.onLine) return;
  
  const db = await initOfflineDB();
  const tx = db.transaction('pendingSync', 'readonly');
  const store = tx.objectStore('pendingSync');
  
  const pending = await store.getAll();
  
  for(const item of pending){
    try{
      await fetch(item.endpoint, {
        method: item.method,
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(item.data)
      });
      
      // Remove from pending
      const deleteTx = db.transaction('pendingSync', 'readwrite');
      await deleteTx.objectStore('pendingSync').delete(item.id);
    } catch(e){
      console.error('Sync failed:', e);
    }
  }
}

// Auto-sync every 30 seconds when online
setInterval(() => {
  if(navigator.onLine) syncWithBackend();
}, 30000);
```

---

## Security & Privacy

### Data Encryption
```javascript
// Encrypt sensitive patient data before storage
async function encryptData(data){
  const key = await getEncryptionKey();
  const iv = crypto.getRandomValues(new Uint8Array(12));
  
  const encrypted = await crypto.subtle.encrypt(
    { name: 'AES-GCM', iv },
    key,
    new TextEncoder().encode(JSON.stringify(data))
  );
  
  return {
    encrypted: btoa(String.fromCharCode(...new Uint8Array(encrypted))),
    iv: btoa(String.fromCharCode(...iv))
  };
}

async function decryptData(encryptedData, iv){
  const key = await getEncryptionKey();
  
  const decrypted = await crypto.subtle.decrypt(
    { name: 'AES-GCM', iv: Uint8Array.from(atob(iv), c => c.charCodeAt(0)) },
    key,
    Uint8Array.from(atob(encryptedData), c => c.charCodeAt(0))
  );
  
  return JSON.parse(new TextDecoder().decode(decrypted));
}
```

---

Ready to implement this architecture? 🚀
