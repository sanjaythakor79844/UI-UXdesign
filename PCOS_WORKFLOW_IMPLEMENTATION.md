# PCOS Workflow Implementation - Based on Conversational Script Guide

## Workflow Structure

### Phase 1: Opening (Privacy Reassurance)
```
AAHA: "From what you've said, I'd like to understand a little more about your 
       cycle and some changes you may have noticed. It's all private, and we'll 
       go at your pace. Okay to start?"

[WAIT FOR RESPONSE]
```

### Phase 2: Menstrual History
```
Q1: "Let's start with your periods — do they come every month, or do they 
     skip or come late?"

     IF skips → "Roughly how often — every couple of months, or longer gaps?"
     IF stopped → "When did you last have one?"
```

### Phase 3: Weight Changes
```
Q2: "Have you gained weight lately, especially around your middle, that's 
     hard to shift?"

     IF yes → "Has it been recent, or building up over a year or two?"
```

### Phase 4: Hirsutism (Sensitive - Reassurance First)
```
[Gentle tone]
Q3: "Have you noticed extra hair growing where you'd rather not — on your 
     face, chin, or body?"

     IF yes → "Is that new, or has it slowly increased?"
```

### Phase 5: Skin Issues
```
Q4: "How's your skin — do you get acne or oily skin that keeps coming back?"

     IF yes → "Mostly around your jaw and chin, or all over?"
```

### Phase 6: Hair Thinning
```
Q5: "And the hair on your head — is it thinning or falling more than usual?"
```

### Phase 7: Acanthosis Nigricans
```
Q6: "Have you noticed any dark, velvety patches of skin — maybe on your 
     neck or underarms?"
```

### Phase 8: Energy & Cravings
```
Q7: "How's your energy through the day — do you feel drained or get strong 
     sugar cravings, especially after meals?"
```

### Phase 9: Thyroid Screening
```
Q8: "A few of these can also come from the thyroid, so let me check — do you 
     often feel unusually cold or tired, or noticed any swelling in your neck?"

     IF neck swelling → "Can you feel it when you swallow, or did someone 
                         point it out?"
```

### Phase 10: Family History
```
Q9: "Does anyone in your family have PCOS, thyroid trouble, or diabetes?"
```

### Phase 11: Diet Assessment (Critical for PCOS)
```
— diet —

Q10: "Now a little about your food — are you mostly vegetarian, or do you 
      eat eggs, chicken, fish too?"

Q11: "On a usual day, do you eat regular meals, or do they get skipped or rushed?"
      IF skips → "Which one usually gets missed?"

Q12: "What does your plate usually look like — mostly rice or roti, with dal 
      or vegetables?"

Q13: "Do you take a lot of sweets, sugary tea, or cold drinks through the day?"
      IF yes → "Is it more a daily habit, or mostly on occasions?"

Q14: "And fried or outside food — how often in a week?"
```

### Phase 12: Into the Tests
```
AAHA: "Thank you for being open with me. Now let's do a few quick checks — 
       your height and weight, blood pressure, and a small blood test. 
       Aasha didi will help, no rush."
```

### Phase 13: Individual Test Consent (For Each Test)
```
EXAMPLE: Hemoglobin Test

AAHA: "Now I'd like to check your haemoglobin — it's just a tiny prick on 
       your fingertip, takes a minute. Aasha didi will do it for you, no 
       hurry. Shall we do it now?"

[WAIT]

IF yes → [Take reading] → "Done, thank you."

IF no → "That's alright. Do you want to try in a bit, come back another 
         day, or carry on for now?"
         
         Options:
         - Try later (wait 5 min, ask again)
         - Reschedule (date picker)
         - Skip for now (continue assessment)
```

### Phase 14: Closing
```
AAHA: "Once we're done I'll put it together in simple words and we'll talk 
       about the next step. PCOS is very manageable once we understand it."
```

---

## Implementation in Code

### State Management
```javascript
state.pcosWorkflow = {
  phase: 'opening',
  currentQuestion: 0,
  responses: {},
  dietInfo: {},
  followUpNeeded: false,
  testsCompleted: [],
  testsPending: []
};
```

### Question Flow Array
```javascript
const PCOS_QUESTIONS = [
  {
    id: 'menstrual',
    text: "Let's start with your periods — do they come every month, or do they skip or come late?",
    followUp: {
      'skip': "Roughly how often — every couple of months, or longer gaps?",
      'stopped': "When did you last have one?"
    },
    voiceHindi: "Chaliye aapke periods se shuru karte hain — kya har mahine aate hain, ya skip hote hain?"
  },
  {
    id: 'weight',
    text: "Have you gained weight lately, especially around your middle, that's hard to shift?",
    followUp: {
      'yes': "Has it been recent, or building up over a year or two?"
    },
    voiceHindi: "Kya aapka weight badha hai, khaas kar pet ke aas-paas?"
  },
  {
    id: 'hirsutism',
    text: "Have you noticed extra hair growing where you'd rather not — on your face, chin, or body?",
    sensitive: true,
    reassurance: "[gentle tone, pause]",
    followUp: {
      'yes': "Is that new, or has it slowly increased?"
    },
    voiceHindi: "Kya aapko face, chin ya body pe extra baal aane lage hain?"
  },
  {
    id: 'acne',
    text: "How's your skin — do you get acne or oily skin that keeps coming back?",
    followUp: {
      'yes': "Mostly around your jaw and chin, or all over?"
    },
    voiceHindi: "Aapki skin kaisi hai — acne ya oily skin wapas aati rehti hai?"
  },
  {
    id: 'hairfall',
    text: "And the hair on your head — is it thinning or falling more than usual?",
    voiceHindi: "Aur sir ke baal — kya patli ho rahi hain ya zyada gir rahi hain?"
  },
  {
    id: 'darkpatches',
    text: "Have you noticed any dark, velvety patches of skin — maybe on your neck or underarms?",
    voiceHindi: "Kya aapne skin pe kaale patches dekhe hain — gardan ya underarms pe?"
  },
  {
    id: 'energy',
    text: "How's your energy through the day — do you feel drained or get strong sugar cravings, especially after meals?",
    voiceHindi: "Din bhar aapki energy kaisi rehti hai — kamzori mehsoos hoti hai ya meetha khaane ka mann karta hai?"
  },
  {
    id: 'thyroid',
    text: "A few of these can also come from the thyroid, so let me check — do you often feel unusually cold or tired, or noticed any swelling in your neck?",
    followUp: {
      'swelling': "Can you feel it when you swallow, or did someone point it out?"
    },
    voiceHindi: "Ye symptoms thyroid se bhi ho sakte hain — kya aapko bahut thandi lagti hai ya gardan mein swelling hai?"
  },
  {
    id: 'family',
    text: "Does anyone in your family have PCOS, thyroid trouble, or diabetes?",
    voiceHindi: "Kya aapke ghar mein kisi ko PCOS, thyroid ya diabetes hai?"
  }
];

const PCOS_DIET_QUESTIONS = [
  {
    id: 'diet_type',
    text: "Now a little about your food — are you mostly vegetarian, or do you eat eggs, chicken, fish too?",
    voiceHindi: "Ab thoda aapke khane ke baare mein — kya aap vegetarian hain, ya anda, chicken, machhli khaate hain?"
  },
  {
    id: 'diet_meals',
    text: "On a usual day, do you eat regular meals, or do they get skipped or rushed?",
    followUp: {
      'skip': "Which one usually gets missed?"
    },
    voiceHindi: "Aam din mein aap regular khana khaate hain, ya skip hota hai?"
  },
  {
    id: 'diet_plate',
    text: "What does your plate usually look like — mostly rice or roti, with dal or vegetables?",
    voiceHindi: "Aapki plate mein usually kya hota hai — zyada chawal ya roti, dal ya sabzi?"
  },
  {
    id: 'diet_sugar',
    text: "Do you take a lot of sweets, sugary tea, or cold drinks through the day?",
    followUp: {
      'yes': "Is it more a daily habit, or mostly on occasions?"
    },
    voiceHindi: "Kya aap din mein bahut meetha, cheeni wali chai ya cold drinks lete hain?"
  },
  {
    id: 'diet_fried',
    text: "And fried or outside food — how often in a week?",
    voiceHindi: "Aur fried ya bahar ka khana — hafte mein kitni baar?"
  }
];
```

### Chat Flow Logic
```javascript
async function runPCOSWorkflow(){
  // Opening with reassurance
  const opening = "From what you've said, I'd like to understand a little more about your cycle and some changes you may have noticed. It's all private, and we'll go at your pace. Okay to start?";
  
  addBubbleWithEyes('ai', opening);
  speak(opening);
  
  // Wait for user consent
  // Then start question-by-question flow
  
  for(let i = 0; i < PCOS_QUESTIONS.length; i++){
    const q = PCOS_QUESTIONS[i];
    
    // Sensitive question? Add gentle pause
    if(q.sensitive){
      await pause(500);
    }
    
    // Ask question
    addBubbleWithEyes('ai', q.text);
    speak(q.voiceHindi || q.text);
    
    // WAIT for response (critical - one at a time!)
    const response = await waitForUserResponse();
    
    // Store response
    state.pcosWorkflow.responses[q.id] = response;
    
    // Check if follow-up needed
    if(q.followUp && q.followUp[response.toLowerCase()]){
      const followUpText = q.followUp[response.toLowerCase()];
      addBubbleWithEyes('ai', followUpText);
      speak(followUpText);
      
      const followUpResponse = await waitForUserResponse();
      state.pcosWorkflow.responses[q.id + '_followup'] = followUpResponse;
    }
  }
  
  // Now diet questions
  const dietIntro = "Now a little about your food...";
  addBubbleWithEyes('ai', dietIntro);
  
  for(let i = 0; i < PCOS_DIET_QUESTIONS.length; i++){
    const q = PCOS_DIET_QUESTIONS[i];
    
    addBubbleWithEyes('ai', q.text);
    speak(q.voiceHindi || q.text);
    
    const response = await waitForUserResponse();
    state.pcosWorkflow.dietInfo[q.id] = response;
    
    // Follow-ups
    if(q.followUp && q.followUp[response.toLowerCase()]){
      const followUpText = q.followUp[response.toLowerCase()];
      addBubbleWithEyes('ai', followUpText);
      speak(followUpText);
      
      const followUpResponse = await waitForUserResponse();
      state.pcosWorkflow.dietInfo[q.id + '_followup'] = followUpResponse;
    }
  }
  
  // Transition to tests
  const testIntro = "Thank you for being open with me. Now let's do a few quick checks — your height and weight, blood pressure, and a small blood test. Aasha didi will help, no rush.";
  
  addBubbleWithEyes('ai', testIntro);
  speak("Dhanyavaad. Ab kuch quick checks karte hain — height, weight, BP aur blood test.");
  
  // Start individual test consent flow
  await startIndividualTestFlow();
}
```

### Individual Test Consent (Per Test)
```javascript
async function askTestConsent(test){
  const consentMessages = {
    hb: "Now I'd like to check your haemoglobin — it's just a tiny prick on your fingertip, takes a minute. Aasha didi will do it for you, no hurry. Shall we do it now?",
    glucose: "Let me check your blood sugar now — same tiny prick. May I go ahead?",
    bp: "I'd like to take your blood pressure — just a cuff on your arm for 30 seconds. Shall we?",
    height_weight: "First, let's check your height and weight — just step on the scale please.",
    thyroid: "Since thyroid symptoms came up, I'd like to do a thyroid test too — another small blood sample. Okay?"
  };
  
  // Show AAHA eyes in center
  showAAHAEyes();
  
  const message = consentMessages[test.id];
  addBubble('ai', message);
  speak(message);
  
  // Wait for response
  const response = await waitForUserResponse();
  
  if(response.toLowerCase().includes('yes') || response.toLowerCase().includes('haan')){
    // User consented - run test
    await runTest(test);
    
    addBubble('ai', "Done, thank you.");
    speak("Ho gaya, dhanyavaad.");
    
    return 'completed';
  } else {
    // User declined
    const declineOptions = "That's alright. Do you want to try in a bit, come back another day, or carry on for now?";
    addBubble('ai', declineOptions);
    speak(declineOptions);
    
    // Show options: Try Later | Reschedule | Skip
    showTestDeclineOptions(test);
    
    return 'declined';
  }
}
```

### Final Report with Diet Advice
```javascript
function buildPCOSReport(){
  const responses = state.pcosWorkflow.responses;
  const diet = state.pcosWorkflow.dietInfo;
  
  // Analyze responses
  const score = calculatePCOSLikelihood(responses);
  
  // Build report sections
  const report = {
    summary: generatePCOSSummary(responses),
    testResults: state.testResults,
    dietAdvice: generatePCOSDietAdvice(diet, responses),
    nextSteps: generatePCOSNextSteps(score)
  };
  
  // Closing message
  const closing = "Once we're done I'll put it together in simple words and we'll talk about the next step. PCOS is very manageable once we understand it.";
  
  speak(closing);
  
  return report;
}

function generatePCOSDietAdvice(diet, symptoms){
  // Personalized based on their actual eating habits
  const advice = [];
  
  // If skips meals (especially breakfast)
  if(diet.diet_meals?.includes('skip')){
    advice.push("Don't skip meals, especially breakfast — it helps control sugar cravings and balances hormones.");
  }
  
  // If high sugar intake
  if(diet.diet_sugar?.toLowerCase().includes('yes')){
    advice.push("Reduce sugary tea, cold drinks, and sweets — PCOS makes your body sensitive to sugar spikes.");
  }
  
  // If mostly rice/roti without protein
  if(diet.diet_plate){
    advice.push("Add protein to every meal — dal, sprouts, paneer, egg if you take it. This slows sugar absorption.");
  }
  
  // If high fried food
  if(diet.diet_fried){
    advice.push("Cut down fried and outside food — aim for 2-3 times a week maximum instead of daily.");
  }
  
  // General PCOS diet principles
  advice.push("Fill half your plate with vegetables — they help manage weight and insulin.");
  advice.push("Choose whole grains over refined — brown rice, millets, whole wheat.");
  
  return advice;
}
```

---

## Key Differences from Current Implementation

| Current | Script-Based (New) |
|---------|-------------------|
| Multiple questions at once | ONE question at a time |
| Generic AI responses | Structured, scripted conversation |
| No diet assessment | Diet woven into workflow |
| Single test consent screen | Individual consent per test |
| Generic report | Personalized based on diet + symptoms |
| No follow-up logic | Context-aware follow-ups |
| Auto-flow | User-paced, respectful |

---

## Next Steps

1. ✅ Implement PCOS question array
2. ✅ Add one-at-a-time chat logic
3. ✅ Implement follow-up detection
4. ✅ Add diet assessment phase
5. ✅ Update test consent to per-test model
6. ✅ Build personalized diet advice generator
7. ✅ Add decline options (try later/reschedule/skip)
8. ✅ Update final report with diet section

Ready to implement? This will make AAHA feel like a real, thoughtful healthcare assistant!
