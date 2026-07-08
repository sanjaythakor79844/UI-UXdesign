# Enhanced Test Flow & Error Handling - Implementation Plan

## 1. CONSISTENT AAHA EYES DESIGN

### Color Palette (from intro/outro)
```css
--eye-bg: #FFFFFF
--eye-border: #2563EB (primary blue)
--pupil-color: #0F172A (dark)
--iris-gradient: radial-gradient(circle, #3B82F6 0%, #1E40AF 50%, #1E3A8A 100%)
--glow: rgba(37, 99, 235, 0.2)
```

### Usage Locations
- ✅ Splash screen
- ✅ Intro video screen  
- ✅ Registration success
- ✅ End/farewell screen
- ✅ ASHA welcome screen
- 🆕 Test consent screens
- 🆕 Test explanation screens
- 🆕 During test execution
- 🆕 Mini result display

---

## 2. INDIVIDUAL TEST FLOW (Per Diagnostic Module)

### Current Flow:
```
Device Selection → All Tests Together → Processing → Final Report
```

### New Enhanced Flow:
```
Device Selection → For Each Selected Test:
  1. Show AAHA (centered, blinking eyes)
  2. Ask Consent ("May I check your Blood Pressure?")
  3. Explain Test (with AAHA visible)
  4. Run Test (AAHA thinking/processing)
  5. Show MINI RESULT (icon + status)
  6. Move to Next Test
→ Final Comprehensive Report
```

### Example Test Sequence:

#### Test 1: Blood Pressure
```
┌─────────────────────────────────┐
│     👁️👁️ AAHA (centered)        │
│                                  │
│  "May I check your Blood         │
│   Pressure now? Please place     │
│   your arm in the cuff."         │
│                                  │
│  [✓ Yes, Continue] [⏭ Skip]     │
└─────────────────────────────────┘

↓ (User consents)

┌─────────────────────────────────┐
│     👁️👁️ AAHA                    │
│                                  │
│  "Blood pressure measures the    │
│   force of blood against artery  │
│   walls. Normal range is 120/80" │
│                                  │
│   [▶ Start Test]                 │
└─────────────────────────────────┘

↓ (Test running - 30 seconds)

┌─────────────────────────────────┐
│     👁️👁️ AAHA (thinking)        │
│                                  │
│  Testing Blood Pressure...       │
│  ████████░░░░░░ 60%              │
│                                  │
│  Please remain still             │
└─────────────────────────────────┘

↓ (Test complete)

┌─────────────────────────────────┐
│  ✅ Blood Pressure Result        │
│                                  │
│     📊 124/82 mmHg               │
│     ⚠️ SLIGHTLY ELEVATED         │
│                                  │
│  "Your BP is slightly high.      │
│   Monitor it regularly."         │
│                                  │
│   [✓ Continue to Next Test]     │
└─────────────────────────────────┘
```

#### Test 2: ECG
```
Similar flow:
Consent → Explain → Run → Mini Result → Next
```

---

## 3. ERROR HANDLING SYSTEM

### A. Machine/Hardware Error (Device Malfunction)

```
┌─────────────────────────────────┐
│     ⚠️ Test Issue                │
│     👁️👁️ AAHA (concerned)       │
│                                  │
│  "The Blood Pressure device is   │
│   not responding properly."      │
│                                  │
│  What would you like to do?      │
│                                  │
│  [🔄 Retry Now]                  │
│  [⏭ Skip This Test]              │
│  [📅 Reschedule Visit]           │
└─────────────────────────────────┘
```

**Option 1: Retry Now**
- Attempt test again immediately
- Max 2 retries
- If still fails → offer Skip or Reschedule

**Option 2: Skip This Test**
- Mark test as "Not Performed - Device Error"
- Continue with remaining tests
- Note in final report

**Option 3: Reschedule Visit**
```
┌─────────────────────────────────┐
│  📅 Reschedule Your Visit        │
│                                  │
│  Select Date:                    │
│  [Date Picker]                   │
│                                  │
│  Select Time Slot:               │
│  ○ 9:00 AM - 11:00 AM            │
│  ○ 11:00 AM - 1:00 PM            │
│  ○ 2:00 PM - 4:00 PM             │
│  ○ 4:00 PM - 6:00 PM             │
│                                  │
│  [Confirm] [Cancel]              │
└─────────────────────────────────┘

↓ (Confirmed)

- Save partial data
- SMS/WhatsApp reminder sent
- Return to home screen
```

### B. Cloud/API Error (Backend Issue)

Same UX as hardware error:
```
┌─────────────────────────────────┐
│     ⚠️ Connection Issue          │
│     👁️👁️ AAHA (concerned)       │
│                                  │
│  "Unable to process test data    │
│   due to connectivity issue."    │
│                                  │
│  [🔄 Retry] [⏭ Skip] [📅 Later] │
└─────────────────────────────────┘
```

---

## 4. MINI RESULT CARDS (After Each Test)

### Template:
```html
<div class="mini-result-card">
  <div class="result-header">
    <span class="result-icon">[ICON]</span>
    <h3>[TEST NAME] Result</h3>
  </div>
  
  <div class="result-value">
    [VALUE] [UNIT]
  </div>
  
  <div class="result-status [status-class]">
    [ICON] [STATUS TEXT]
  </div>
  
  <div class="result-note">
    [BRIEF EXPLANATION]
  </div>
  
  <button class="next-btn">Continue to Next Test →</button>
</div>
```

### Status Types:
- ✅ **Normal**: Green - "Within healthy range"
- ⚠️ **Monitor**: Yellow - "Slightly elevated - monitor regularly"  
- 🚨 **High/Low**: Red - "Requires doctor consultation"
- ℹ️ **Info**: Blue - "Test completed successfully"

### Examples:

**Blood Sugar (Normal)**
```
┌─────────────────────────────────┐
│  ✅ Blood Sugar Result           │
│                                  │
│     🩸 94 mg/dL                  │
│     ✓ NORMAL RANGE               │
│                                  │
│  Your blood sugar is healthy.    │
│  Keep maintaining your diet!     │
│                                  │
│   [Continue →]                   │
└─────────────────────────────────┘
```

**Hemoglobin (Low)**
```
┌─────────────────────────────────┐
│  🚨 Hemoglobin Result            │
│                                  │
│     🩸 9.2 g/dL                  │
│     ⚠️ LOW - ANEMIA RISK         │
│                                  │
│  Your hemoglobin is low.         │
│  Increase iron-rich foods.       │
│                                  │
│   [Continue →]                   │
└─────────────────────────────────┘
```

**Estrogen (High)**
```
┌─────────────────────────────────┐
│  ⚠️ Estrogen Result              │
│                                  │
│     🧬 182 pg/mL                 │
│     ⚠️ ELEVATED                  │
│                                  │
│  Your estrogen is higher than    │
│  normal. Consult gynecologist.   │
│                                  │
│   [Continue →]                   │
└─────────────────────────────────┘
```

---

## 5. FINAL COMPREHENSIVE REPORT

After all tests complete:
```
┌─────────────────────────────────┐
│  🏥 Complete Health Report       │
│                                  │
│  Patient: [Name]                 │
│  Tests: 8 completed, 1 skipped   │
│                                  │
│  🎯 Wellness Score: 72/100       │
│  ⚠️ Needs Attention               │
│                                  │
│  📊 All Test Results:            │
│  ├─ Blood Pressure: ⚠️ High      │
│  ├─ Blood Sugar: ✅ Normal        │
│  ├─ Hemoglobin: 🚨 Low           │
│  ├─ ECG: ✅ Normal                │
│  ├─ Estrogen: ⚠️ High            │
│  └─ Thyroid: [Skipped - Device]  │
│                                  │
│  💊 Recommendations...           │
│  [Download] [WhatsApp] [Print]   │
└─────────────────────────────────┘
```

---

## 6. IMPLEMENTATION CHECKLIST

### Phase 1: AAHA Eyes Component
- [ ] Create reusable `renderAahaEyes()` component
- [ ] Standardize color palette
- [ ] Add to all consent screens
- [ ] Add to explanation screens
- [ ] Add to processing screens

### Phase 2: Individual Test Flow
- [ ] Break device screen into per-test consent
- [ ] Add test explanation modal
- [ ] Create mini-result card component
- [ ] Implement test sequencing logic

### Phase 3: Error Handling
- [ ] Add retry mechanism (max 2 attempts)
- [ ] Create skip test flow
- [ ] Build reschedule UI (date/time picker)
- [ ] Save partial progress
- [ ] SMS/WhatsApp notification for reschedule

### Phase 4: Mini Results
- [ ] Design result card templates
- [ ] Add status color system
- [ ] Write brief explanations for each test
- [ ] Implement "Continue" flow

### Phase 5: Enhanced Final Report
- [ ] Add "Tests Completed/Skipped" summary
- [ ] List all mini results
- [ ] Note any device errors
- [ ] Include reschedule info if needed

---

## 7. TEST-SPECIFIC DETAILS

### Available Diagnostic Modules:
1. **Blood Pressure** - 30 sec - Range: 90/60 to 120/80
2. **Pulse Oximeter (SpO2)** - 15 sec - Range: 95-100%
3. **ECG** - 45 sec - Rhythm analysis
4. **Glucometer (Blood Sugar)** - 20 sec - Range: 70-100 mg/dL fasting
5. **Thermometer** - 10 sec - Range: 97-99°F
6. **Hemoglobin** - 60 sec - Range: 12-16 g/dL (women), 14-18 g/dL (men)
7. **Cholesterol** - 120 sec - Range: <200 mg/dL total
8. **Hormone Panel** (Estrogen, Testosterone, Thyroid) - 180 sec each

### Consent Messages:
```javascript
const CONSENT_MESSAGES = {
  bp: "May I check your Blood Pressure? Please place your arm in the cuff.",
  spo2: "May I check your Blood Oxygen level? Please place your finger in the sensor.",
  ecg: "May I perform an ECG test? Please attach the electrodes to your chest.",
  glucose: "May I check your Blood Sugar? A small finger prick will be needed.",
  temp: "May I check your Temperature? Please place the sensor under your tongue.",
  hemoglobin: "May I check your Hemoglobin? A small blood sample will be needed.",
  cholesterol: "May I check your Cholesterol levels? A blood sample will be needed.",
  estrogen: "May I check your Estrogen level? A blood sample will be needed."
};
```

### Explanation Messages:
```javascript
const EXPLANATIONS = {
  bp: "Blood pressure measures the force of blood against artery walls. Normal is 120/80 mmHg.",
  spo2: "SpO2 shows how much oxygen your blood is carrying. Normal is 95-100%.",
  ecg: "ECG records your heart's electrical activity to detect rhythm issues.",
  glucose: "Blood sugar test checks glucose levels. Normal fasting is 70-100 mg/dL.",
  // ... etc
};
```

---

## 8. VOICE INTEGRATION

AAHA should speak during:
1. ✅ Consent request
2. ✅ Test explanation  
3. ✅ Test instructions ("Please remain still")
4. ✅ Mini result announcement
5. ✅ Error messages
6. ✅ Retry/reschedule options

---

## ESTIMATED TIMELINE
- Phase 1 (Eyes): 2 hours
- Phase 2 (Test Flow): 4 hours
- Phase 3 (Error Handling): 3 hours
- Phase 4 (Mini Results): 2 hours
- Phase 5 (Final Report): 2 hours
- Testing & Polish: 2 hours

**Total: ~15 hours of work**

Ready to start implementation?
