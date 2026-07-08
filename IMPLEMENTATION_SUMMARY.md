# PCOS Workflow Implementation - Complete Summary

## ✅ What's Been Done

### 1. Backend Architecture Document (`BACKEND_ARCHITECTURE.md`)
- Complete API endpoint specifications
- Database models (PostgreSQL + MongoDB)
- Workflow Service architecture
- Security & encryption layer
- Offline sync capability with IndexedDB

### 2. PCOS Workflow Definition (`workflows/pcos-workflow.json`)
- Complete 9-question symptom assessment
- 5-question diet assessment
- Follow-up logic for each question
- Scoring algorithm with thresholds
- 5 clinical tests with consent messages
- Personalized diet rules based on responses

### 3. Frontend Enhancements Already Done
- ✅ Individual test flow with consent
- ✅ Mini test results
- ✅ Error handling (Retry/Skip/Reschedule)
- ✅ AAHA eyes with intro/outro color palette
- ✅ Eyes appearing in chat during conversation
- ✅ Enhanced final report with wellness score

---

## 🔄 What Needs to Be Added (Next Phase)

### Frontend Workflow Engine

Add this new JavaScript module to `index.html` before `</script>`:

```javascript
/* ============================= WORKFLOW ENGINE ============================= */

// Workflow state management
const workflowEngine = {
  active: false,
  sessionId: null,
  workflowType: null,
  definition: null,
  currentPhase: null,
  currentQuestionIndex: 0,
  responses: {},
  dietInfo: {},
  testResults: {},
  score: null,
  riskLevel: null,
  awaitingResponse: false,
  
  // Load workflow from JSON
  async loadWorkflow(type){
    const response = await fetch(`workflows/${type}-workflow.json`);
    return await response.json();
  },
  
  // Start workflow
  async start(type){
    this.workflowType = type;
    this.sessionId = generateUUID();
    this.definition = await this.loadWorkflow(type);
    this.currentPhase = this.definition.phases[0];
    this.active = true;
    
    console.log(`[Workflow] Started ${type} workflow`, this.sessionId);
    
    // Execute opening phase
    await this.executePhase(this.currentPhase);
  },
  
  // Execute a phase
  async executePhase(phase){
    console.log(`[Workflow] Executing phase: ${phase.phaseId}`);
    
    switch(phase.type){
      case 'consent':
        await this.showConsent(phase);
        break;
      case 'questionnaire':
        await this.runQuestionnaire(phase);
        break;
      case 'clinical':
        await this.runTests(phase);
        break;
      case 'summary':
        await this.showSummary(phase);
        break;
    }
  },
  
  // Show consent message
  async showConsent(phase){
    const message = phase.message[state.language] || phase.message.en;
    addBubbleWithAAHAEyes('ai', message);
    await speak(message);
    
    // Wait for user consent
    this.awaitingResponse = true;
    const consent = await waitForUserInput();
    this.awaitingResponse = false;
    
    if(consent.toLowerCase().includes('yes') || consent.toLowerCase().includes('haan') || consent.toLowerCase().includes('ok')){
      // Move to next phase
      this.moveToNextPhase();
    } else {
      // User declined - end workflow
      addBubble('ai', "That's okay. Come back anytime you're ready.");
      speak("Koi baat nahi. Jab aap ready hain tab wapas aaiye.");
      this.active = false;
    }
  },
  
  // Run questionnaire phase
  async runQuestionnaire(phase){
    if(phase.intro){
      const intro = phase.intro[state.language] || phase.intro.en;
      addBubbleWithAAHAEyes('ai', intro);
      await speak(intro);
      await sleep(1000);
    }
    
    // Ask questions one by one
    for(let i = 0; i < phase.questions.length; i++){
      const question = phase.questions[i];
      await this.askQuestion(question, phase.phaseId);
    }
    
    // Phase complete - move to next
    this.moveToNextPhase();
  },
  
  // Ask a single question
  async askQuestion(question, phaseId){
    const { id, text, type, sensitive, followUp } = question;
    
    // Gentle pause for sensitive questions
    if(sensitive){
      await sleep(500);
    }
    
    // Show AAHA eyes
    const questionText = text[state.language] || text.en;
    addBubbleWithAAHAEyes('ai', questionText);
    await speak(questionText);
    
    // Wait for response
    this.awaitingResponse = true;
    const response = await waitForUserInput();
    this.awaitingResponse = false;
    
    // Store response
    const storageKey = phaseId === 'diet' ? 'dietInfo' : 'responses';
    this[storageKey][id] = {
      answer: response,
      timestamp: new Date().toISOString()
    };
    
    // Check for follow-up
    if(followUp){
      const followUpConfig = followUp[response.toLowerCase()];
      if(followUpConfig){
        await this.askFollowUp(id, followUpConfig, storageKey);
      }
    }
    
    // Sync with backend (when available)
    // await this.syncWithBackend('answer', {questionId: id, answer: response});
  },
  
  // Ask follow-up question
  async askFollowUp(parentId, followUpConfig, storageKey){
    const followUpText = followUpConfig.text[state.language] || followUpConfig.text.en;
    addBubbleWithAAHAEyes('ai', followUpText);
    await speak(followUpText);
    
    this.awaitingResponse = true;
    const response = await waitForUserInput();
    this.awaitingResponse = false;
    
    // Store follow-up
    if(!this[storageKey][parentId].followUp){
      this[storageKey][parentId].followUp = {};
    }
    this[storageKey][parentId].followUp = response;
  },
  
  // Run clinical tests
  async runTests(phase){
    const intro = phase.intro[state.language] || phase.intro.en;
    addBubbleWithAAHAEyes('ai', intro);
    await speak(intro);
    await sleep(1500);
    
    // Filter tests based on conditions
    const testsToRun = phase.tests.filter(test => {
      if(!test.conditional) return true;
      
      // Check if conditional test should run
      const condition = test.conditional;
      const response = this.responses[condition.questionId];
      return response && condition.values.includes(response.answer.toLowerCase());
    });
    
    // Run each test with individual consent
    for(const test of testsToRun){
      await this.runSingleTest(test);
    }
    
    // Tests complete - move to closing
    this.moveToNextPhase();
  },
  
  // Run a single test with consent
  async runSingleTest(test){
    const consentMessage = test.consentMessage[state.language] || test.consentMessage.en;
    
    // Show AAHA eyes and ask consent
    showAAHAEyes();
    addBubble('ai', consentMessage);
    await speak(consentMessage);
    
    this.awaitingResponse = true;
    const consent = await waitForUserInput();
    this.awaitingResponse = false;
    
    if(consent.toLowerCase().includes('yes') || consent.toLowerCase().includes('haan')){
      // User consented - run test using existing test flow
      const deviceObj = DEVICES.find(d => d.id === test.testId);
      if(deviceObj){
        await runTest(deviceObj);
      }
      
      addBubble('ai', "Done, thank you.");
      await speak("Ho gaya, dhanyavaad.");
    } else {
      // User declined
      const declineMsg = "That's alright. Do you want to try in a bit, come back another day, or carry on for now?";
      addBubble('ai', declineMsg);
      await speak(declineMsg);
      
      // Handle decline options (Skip/Reschedule)
      // ... existing decline handling code
    }
  },
  
  // Show summary and generate report
  async showSummary(phase){
    const message = phase.message[state.language] || phase.message.en;
    addBubbleWithAAHAEyes('ai', message);
    await speak(message);
    
    // Calculate score
    this.calculateScore();
    
    // Generate personalized diet advice
    this.generateDietAdvice();
    
    // Build final report
    await this.buildWorkflowReport();
    
    // End workflow
    this.active = false;
    goTo('screen-diagnosis');
  },
  
  // Calculate workflow score
  calculateScore(){
    let totalScore = 0;
    let maxScore = 0;
    
    // Iterate symptom questions
    const symptomPhase = this.definition.phases.find(p => p.phaseId === 'symptoms');
    
    symptomPhase.questions.forEach(q => {
      const response = this.responses[q.id];
      if(response && q.scoringRules){
        const points = q.scoringRules[response.answer.toLowerCase()] || 0;
        totalScore += points;
        maxScore += Math.max(...Object.values(q.scoringRules));
      }
    });
    
    // Determine risk level
    const thresholds = this.definition.scoring.thresholds;
    let riskLevel;
    if(totalScore <= thresholds.low[1]) riskLevel = 'low';
    else if(totalScore <= thresholds.moderate[1]) riskLevel = 'moderate';
    else riskLevel = 'high';
    
    this.score = totalScore;
    this.riskLevel = riskLevel;
    
    console.log(`[Workflow] Score: ${totalScore}/${maxScore}, Risk: ${riskLevel}`);
  },
  
  // Generate personalized diet advice
  generateDietAdvice(){
    const advice = [];
    const rules = this.definition.dietRules;
    
    // Check each diet rule
    Object.keys(rules).forEach(ruleKey => {
      const rule = rules[ruleKey];
      
      if(Array.isArray(rule)){
        // General advice
        advice.push(...rule);
      } else {
        // Conditional advice
        const response = this.dietInfo[rule.questionId];
        if(response && rule.trigger.includes(response.answer.toLowerCase())){
          advice.push(rule.advice);
        }
      }
    });
    
    state.workflowDietAdvice = advice;
    console.log(`[Workflow] Generated ${advice.length} diet recommendations`);
  },
  
  // Build workflow-based report
  async buildWorkflowReport(){
    // Enhance existing buildReport() with workflow data
    state.workflowScore = this.score;
    state.workflowRisk = this.riskLevel;
    state.workflowType = this.workflowType;
    state.workflowResponses = this.responses;
    
    // Call existing report builder
    buildReport();
  },
  
  // Move to next phase
  moveToNextPhase(){
    const phases = this.definition.phases;
    const currentIndex = phases.findIndex(p => p.phaseId === this.currentPhase.phaseId);
    
    if(currentIndex < phases.length - 1){
      this.currentPhase = phases[currentIndex + 1];
      this.executePhase(this.currentPhase);
    } else {
      console.log('[Workflow] All phases complete');
    }
  }
};

// Helper: Add bubble with AAHA eyes
function addBubbleWithAAHAEyes(role, text){
  if(role === 'ai' && state.chatHistory.length > 0){
    // Show eyes before AI message
    showAAHAEyesInChat();
  }
  addBubble(role, text);
}

// Helper: Show AAHA eyes in chat
function showAAHAEyesInChat(){
  const scroll = $('#chat-scroll');
  const aahaAvatar = document.createElement('div');
  aahaAvatar.className = 'chat-aaha-avatar';
  aahaAvatar.innerHTML = `
    <div class="aaha-eyes-container">
      <div class="aaha-eye left-eye"></div>
      <div class="aaha-eye right-eye"></div>
    </div>
    <div class="chat-aaha-status">AAHA is thinking...</div>
  `;
  scroll.appendChild(aahaAvatar);
  scroll.scrollTop = scroll.scrollHeight;
  
  setTimeout(() => {
    const status = aahaAvatar.querySelector('.chat-aaha-status');
    if(status) status.textContent = 'AAHA';
  }, 800);
}

// Helper: Wait for user input
function waitForUserInput(){
  return new Promise(resolve => {
    const checkInterval = setInterval(() => {
      if(!workflowEngine.awaitingResponse){
        clearInterval(checkInterval);
        // Get last user message
        const lastUserMsg = state.chatHistory[state.chatHistory.length - 1];
        resolve(lastUserMsg.content);
      }
    }, 100);
  });
}

// Helper: Sleep function
function sleep(ms){
  return new Promise(resolve => setTimeout(resolve, ms));
}

// Helper: Generate UUID
function generateUUID(){
  return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, c => {
    const r = Math.random() * 16 | 0;
    const v = c === 'x' ? r : (r & 0x3 | 0x8);
    return v.toString(16);
  });
}
```

### Integration with Existing Chat

Update the `sendChat()` function to detect PCOS and trigger workflow:

```javascript
async function sendChat(text){
  if(!text || !text.trim()) return;
  
  // If workflow active, handle response
  if(workflowEngine.active && workflowEngine.awaitingResponse){
    addBubble('user', text);
    state.chatHistory.push({role:'user', content:text});
    $('#chat-input').value='';
    workflowEngine.awaitingResponse = false;
    return;
  }
  
  addBubble('user', text);
  state.chatHistory.push({role:'user', content:text});
  $('#chat-input').value='';
  setSuggested([]);
  showThinking();
  
  const reply = await callGemini(state.chatHistory);
  hideThinking();
  addBubble('ai', reply);
  state.chatHistory.push({role:'assistant', content:reply});
  speak(reply);
  chatTurns++;
  
  // Detect if PCOS workflow should start
  const conversationText = state.chatHistory.map(m => m.content).join(' ').toLowerCase();
  
  if(/period|menstrual|cycle|pcos|irregular.*period|missed.*period/i.test(conversationText) && chatTurns >= 2){
    // Start PCOS workflow
    speak("Based on what you've shared, let me ask you some specific questions about PCOS. One moment...");
    await sleep(2000);
    await workflowEngine.start('pcos');
    return;
  }
  
  // ... existing test selection logic
}
```

---

## 📊 Current Status

### ✅ Completed
1. Backend architecture design
2. PCOS workflow JSON definition
3. Individual test flow with consent
4. Mini test results
5. Error handling (Retry/Skip/Reschedule)
6. AAHA eyes with consistent design
7. Eyes in chat during conversation

### 🔄 In Progress
1. Workflow engine JavaScript implementation
2. Integration with existing chat system
3. Workflow-based report generation
4. Diet advice personalization

### ⏳ Pending
1. Backend API development (Node.js/Python)
2. Database setup (PostgreSQL + MongoDB)
3. Other workflows (Anaemia, Thyroid, Mental Health, etc.)
4. MCP integration with diagnostic devices
5. WhatsApp/SMS notification system

---

## 🚀 Next Steps

### Immediate (Frontend):
1. Add workflow engine code to index.html
2. Copy pcos-workflow.json to workflows/ folder  
3. Update sendChat() to detect and trigger workflow
4. Test end-to-end PCOS flow

### Short-term (Backend):
1. Set up Node.js/Express or Python/Flask backend
2. Implement workflow API endpoints
3. Set up PostgreSQL database
4. Configure MongoDB for logs

### Long-term:
1. Build other 6 workflows (Anaemia, Thyroid, etc.)
2. Deploy to cloud (AWS/Azure/GCP)
3. Connect real diagnostic devices
4. ASHA worker portal with analytics

---

## 📁 File Structure

```
Aaroogya/
├── index.html (main kiosk UI)
├── workflows/
│   ├── pcos-workflow.json ✅
│   ├── anaemia-workflow.json (future)
│   ├── thyroid-workflow.json (future)
│   └── mental-health-workflow.json (future)
├── backend/ (future)
│   ├── server.js
│   ├── routes/
│   ├── services/
│   └── models/
└── docs/
    ├── BACKEND_ARCHITECTURE.md ✅
    ├── PCOS_WORKFLOW_IMPLEMENTATION.md ✅
    └── IMPLEMENTATION_SUMMARY.md ✅
```

---

**Ready for next phase? Let me know if you want me to add the workflow engine code to index.html now!** 🚀
