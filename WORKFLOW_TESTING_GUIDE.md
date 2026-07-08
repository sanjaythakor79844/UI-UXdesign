# PCOS Workflow Testing Guide

## ✅ Implementation Complete!

All code has been added to `index.html`. The workflow engine is now ready to test.

---

## 🚀 How to Test

### 1. Start Server
```bash
py -m http.server 8080
```

### 2. Open in Browser
```
http://localhost:8080/index.html
```

### 3. Trigger PCOS Workflow

#### Step 1: Complete Registration
- Enter phone number
- Fill patient details (name, age, gender, village)

#### Step 2: Start Chat
The chat screen will open. Type messages that trigger PCOS workflow:

**Trigger Keywords** (any of these will start PCOS workflow after 2-3 messages):
- "My periods are irregular"
- "Period late hai"
- "Cycle skip ho jata hai"
- "PCOS symptoms hai"
- "Menstrual problem hai"

#### Step 3: Example Conversation

**You**: "Mujhe periods irregular hai"

**AAHA**: [Some response]

**You**: "Haan, 2-3 months skip ho jate hain"

**AAHA**: "Based on what you've shared, let me ask you some specific questions. One moment..."

🎉 **PCOS Workflow Starts!**

---

## 📋 What to Expect

### Phase 1: Consent
```
AAHA (with eyes): "From what you've said, I'd like to understand a little 
                   more about your cycle and some changes you may have noticed. 
                   It's all private, and we'll go at your pace. Okay to start?"

You: "Yes" / "Haan" / "Ok"
```

### Phase 2: Symptom Questions (9 questions, one at a time)

**Q1**: "Let's start with your periods — do they come every month, or skip?"
- Type: "skip" or "irregular" or "stopped"
- **Follow-up** (if skip): "Roughly how often — every couple of months?"
- Type: "2-3 months"

**Q2**: "Have you gained weight lately, especially around your middle?"
- Type: "yes" or "no"
- **Follow-up** (if yes): "Has it been recent, or 1-2 years?"
- Type: "1-2 years"

**Q3**: "Have you noticed extra hair growing on face, chin, or body?"
- Type: "yes" or "no"
- **Follow-up** (if yes): "Is that new, or slowly increasing?"
- Type: "slowly increasing"

**Q4**: "How's your skin — acne or oily skin that keeps coming back?"
- Type: "yes" or "no"
- **Follow-up** (if yes): "Mostly around jaw and chin, or all over?"
- Type: "jaw and chin"

**Q5**: "Hair on your head — thinning or falling more than usual?"
- Type: "yes" or "no"

**Q6**: "Dark, velvety patches of skin on neck or underarms?"
- Type: "yes" or "no"

**Q7**: "How's your energy — feel drained or sugar cravings after meals?"
- Type: "very tired" or "sugar cravings" or "feel good"

**Q8**: "Do you feel unusually cold or tired, or swelling in neck?" (thyroid screening)
- Type: "no" or "cold and tired" or "neck swelling"

**Q9**: "Does anyone in family have PCOS, thyroid, or diabetes?"
- Type: "pcos" or "diabetes" or "no" or "multiple"

### Phase 3: Diet Assessment (5 questions)

**AAHA**: "Now a little about your food..."

**Q1**: "Are you mostly vegetarian, or eat eggs, chicken, fish?"
- Type: "vegetarian" or "non veg" or "eggs"

**Q2**: "Do you eat regular meals, or skip them?"
- Type: "skip sometimes" or "regular"
- **Follow-up** (if skip): "Which meal gets missed?"
- Type: "breakfast"

**Q3**: "What does your plate look like — rice or roti, dal or vegetables?"
- Type: "mostly rice with dal"

**Q4**: "Do you take sweets, sugary tea, or cold drinks?"
- Type: "yes" or "no"
- **Follow-up** (if yes): "Daily habit or occasions?"
- Type: "daily"

**Q5**: "Fried or outside food — how often in a week?"
- Type: "3-4 times" or "daily" or "rarely"

### Phase 4: Clinical Tests

**AAHA**: "Thank you. Now let's do a few quick checks..."

Then it will go through individual test consent:
- Height & Weight
- Blood Pressure  
- Blood Glucose
- Hemoglobin
- Thyroid (only if symptoms mentioned)

Each test will show:
1. AAHA eyes in center
2. Consent message
3. Wait for "yes"/"haan"
4. Run test with progress bar
5. Show mini result
6. Move to next test

### Phase 5: Final Report

After all tests, AAHA will:
1. Calculate PCOS risk score (0-30)
2. Generate personalized diet advice based on your responses
3. Show comprehensive report with:
   - Overall Wellness Score
   - PCOS Risk Assessment (Low/Moderate/High)
   - All test results
   - **Personalized Diet Recommendations** (based on your diet responses!)
   - Next steps

---

## 🎯 Testing Checklist

### Basic Flow
- [ ] Workflow triggers after PCOS-related keywords
- [ ] Opening consent shows AAHA eyes
- [ ] Questions appear one at a time
- [ ] Follow-up questions appear when needed
- [ ] AAHA speaks each question in Hindi
- [ ] User responses are stored

### Symptom Assessment
- [ ] All 9 symptom questions asked in order
- [ ] Follow-ups work correctly (e.g., period frequency)
- [ ] Sensitive questions have gentle pauses
- [ ] No questions skipped

### Diet Assessment
- [ ] Diet intro message shows
- [ ] All 5 diet questions asked
- [ ] Follow-ups for meal skipping work
- [ ] Follow-up for sugar frequency works

### Test Flow
- [ ] Transition message to tests
- [ ] Individual consent for each test
- [ ] AAHA eyes visible during consent
- [ ] Tests run with progress bar
- [ ] Mini results show after each test
- [ ] Can skip/retry tests
- [ ] Conditional test (thyroid) only shows if needed

### Final Report
- [ ] PCOS Risk Assessment shown
- [ ] Risk level calculated correctly
- [ ] Personalized Diet Recommendations section appears
- [ ] Diet advice matches user's responses (e.g., "Don't skip breakfast" if they skip)
- [ ] Test results summary included
- [ ] Download/WhatsApp/Print buttons work

---

## 🐛 Troubleshooting

### Workflow Doesn't Start
**Issue**: PCOS keywords not triggering workflow

**Fix**: 
- Type more clearly: "periods irregular" or "PCOS"
- Have at least 2-3 messages in conversation
- Check console: Should see `[Workflow] Started pcos workflow`

### JSON Not Loading
**Issue**: `workflows/pcos-workflow.json` not found

**Fix**:
```bash
# Create workflows folder
mkdir workflows

# Copy the JSON file
# Make sure pcos-workflow.json is in: Aaroogya/workflows/pcos-workflow.json
```

### Questions Not Appearing
**Issue**: Workflow stuck, no questions

**Check**:
1. Open browser console (F12)
2. Look for errors
3. Check: `[Workflow] Executing phase: symptoms`
4. Check: AAHA eyes appearing in chat

### Follow-ups Not Working
**Issue**: Follow-up questions don't appear

**Debug**:
- Type exact words: "skip", "yes", "no", "stopped"
- Case insensitive but spelling matters
- Check console for response storage

### Diet Advice Not Showing
**Issue**: Report shows generic home remedies instead of personalized diet

**Fix**:
- Make sure you completed all diet questions
- Check: `state.workflowDietAdvice` should have items
- Console should show: `[Workflow] Generated X diet recommendations`

---

## 📊 Expected Console Logs

```
[Workflow Engine] Loaded successfully
[Workflow] Started pcos workflow xxxxxxxx-xxxx-xxxx
[Workflow] Executing phase: consent
[Workflow] Executing phase: symptoms
[Workflow] Executing phase: diet
[Workflow] Executing phase: tests
[Workflow] Running 4 tests
[Workflow] Executing phase: closing
[Workflow] Score: 18/30, Risk: moderate
[Workflow] Generated 5 diet recommendations
[Workflow] All phases complete
```

---

## 🎨 Visual Indicators

### AAHA Eyes in Chat
During workflow, you should see:
```
    👁️👁️ (orange gradient eyes)
    "AAHA is thinking..."
```

Before each question.

### One Question at a Time
Only ONE question visible at a time. Wait for response before next question.

### Follow-up Indentation
Follow-up questions appear immediately after trigger response.

---

## 📱 Sample Full Test Run

```
USER: "Mujhe periods problem hai"
AAHA: "Can you tell me more about it?"

USER: "Irregular hai, skip ho jate hain"
AAHA: "Based on what you've shared, let me ask specific questions..."

[PCOS Workflow Starts]

AAHA (with eyes): "From what you've said, I'd like to understand more... Okay to start?"
USER: "yes"

AAHA: "Let's start with your periods — every month or skip?"
USER: "skip"
AAHA: "Roughly how often — couple of months?"
USER: "2-3 months"

AAHA: "Weight gained lately, especially around middle?"
USER: "yes"
AAHA: "Recent or 1-2 years?"
USER: "1 year"

AAHA: "Extra hair on face, chin, or body?"
USER: "yes"
AAHA: "New or slowly increasing?"
USER: "increasing"

... [continues through all 9 symptom questions]

AAHA: "Now about your food..."

AAHA: "Mostly vegetarian or eat eggs, chicken?"
USER: "vegetarian"

AAHA: "Regular meals or skip?"
USER: "skip sometimes"
AAHA: "Which meal missed?"
USER: "breakfast"

... [continues through 5 diet questions]

AAHA: "Thank you. Now let's do checks — height, weight, BP, blood test..."

[Individual test consents with AAHA eyes]

AAHA: "First, height and weight — step on scale please."
USER: "yes"
[Test runs, mini result shows]

AAHA: "I'd like to take your blood pressure..."
USER: "yes"
[Test runs, mini result shows]

... [all tests complete]

AAHA: "Once we're done I'll put it together... PCOS is very manageable..."

[Final Report Generated]

Report shows:
- PCOS Risk: Moderate Risk (Score: 18/30)
- Wellness Score: 72/100
- Test Results Summary
- 🍽️ Personalized Diet Recommendations:
  • "Don't skip meals, especially breakfast"
  • "Reduce sugary tea and cold drinks"
  • "Add protein to every meal — dal, sprouts, paneer"
  • "Fill half your plate with vegetables"
  • etc.
```

---

## ✅ Success Criteria

Workflow is working correctly when:

1. ✅ PCOS keywords trigger workflow
2. ✅ One question at a time
3. ✅ Follow-ups appear when needed
4. ✅ All 14 questions asked (9 symptoms + 5 diet)
5. ✅ AAHA eyes visible during questions
6. ✅ Voice speaks in Hindi
7. ✅ Tests run individually with consent
8. ✅ Score calculated (0-30)
9. ✅ Diet advice personalized
10. ✅ Final report includes everything

---

**Ready to test! Open http://localhost:8080/index.html and try it!** 🚀

If you face any issues, check the console logs and refer to the troubleshooting section above.
