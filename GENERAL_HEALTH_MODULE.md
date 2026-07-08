# General Health Module - Complete Implementation

## 📋 Overview

The General Health check-in is the **front door** of AAHA - it handles everyday complaints and routes patients to specific workflows. Most people don't arrive with a diagnosis; they say "I'm tired" or "my head hurts."

---

## 🎯 Core Concept

### The Flow:
1. **Open listening** - Patient describes complaints in their own words
2. **Follow-up questions** - Detailed exploration of each complaint
3. **Red flag screening** - Check for danger signs (SNOOP criteria)
4. **Baseline checks** - Run essential tests for everyone
5. **Smart routing** - Route to specific workflows based on pattern
6. **AWIS Score** - Unified wellness score from all data
7. **Next steps** - Clear recommendations and follow-ups

---

## 🔍 Common Complaints Reference

### 1. **Headache** (~39% of primary care visits)

**May Point To:**
- Tension headache (most common)
- Eye strain
- High blood pressure
- Sinus issues
- Anaemia
- Thyroid problems
- Dehydration
- Stress

**Follow-up Questions:**
```
- "Kab se ho raha hai?" (Since when?)
- "Roz hota hai, ya kabhi kabhi?" (Daily or sometimes?)
- "Kahan zyada mehsoos hota hai?" (Where do you feel it?)
- "Din mein kab zyada hota hai?" (What time is worse?)
- "Koi dawai li?" (Any medication?)
- "BP ya thyroid ki jaani-maani taklif?" (Known BP/thyroid?)
```

**⚠️ RED FLAGS (SNOOP Criteria) → URGENT REFERRAL:**
- **S**udden 'worst ever' (thunderclap headache)
- **N**eurological signs (weakness, vision loss, confusion)
- **O**nset with cough/straining
- **O**lder than 50 new-onset
- **P**attern change - worse lying down or waking up
- + Fever, neck stiffness, vomiting

**Tests to Run:**
- Blood Pressure (immediate)
- Vision screening
- Haemoglobin (if fatigue/pallor)
- CRP/ESR (if systemic symptoms)

**Routes To:**
- Thyroid workflow (if fatigue-linked)
- Anaemia workflow (if pallor/breathlessness)
- **Urgent referral** (if red flags)

---

### 2. **Tiredness / Fatigue** (~46% of primary care)

**May Point To:**
- Anaemia (very common in India)
- Thyroid disorder (hypo/hyper)
- Diabetes
- Poor sleep/stress
- Low mood/depression
- Poor nutrition
- Chronic infection
- Vitamin D deficiency

**Follow-up Questions:**
```
- "Kab se mehsoos ho rahi hai?" (Since when?)
- "Poori neend ke baad bhi thakaan?" (Tired even after sleep?)
- "Thand zyada lagti hai?" (Feeling cold?)
- "Wajan mein badlaav?" (Weight changes?)
- "Zyada pyaas, baar baar peshaab?" (Thirst, frequent urination?)
- "Neend aur mann kaisa?" (Sleep and mood?)
- "Koi dawai li?" (Any medication?)
```

**⚠️ RED FLAGS → URGENT:**
- Rapid unintentional weight loss
- Drenching night sweats
- Breathlessness at rest
- Chest pain
- Confusion/drowsiness

**Tests to Run:**
- Haemoglobin/CBC (anaemia)
- TSH (thyroid)
- Blood sugar/HbA1c (diabetes)
- Vitamin D
- CRP/ESR (inflammation)

**Routes To:**
- **Anaemia workflow** (if low Hb/pallor)
- **Thyroid workflow** (if cold intolerance/weight gain)
- **Metabolic/Diabetes workflow** (if thirst/urination)
- Mental health support (if mood persistent)

---

### 3. **Joint / Body Ache**

**May Point To:**
- Mechanical strain/overuse
- Vitamin D deficiency (very common)
- Inflammatory arthritis
- Viral illness
- Fibromyalgia

**Follow-up Questions:**
```
- "Kaun se joint?" (Which joints?)
- "Subah akdapan?" (Morning stiffness?)
- "Soojan hai?" (Swelling?)
- "Chot lagi thi?" (Any trauma?)
- "Kaam mein zyada strain?" (Work strain?)
```

**⚠️ RED FLAGS → URGENT:**
- Hot swollen joint + fever
- Night pain/rest pain
- Weight loss
- Leg weakness/inability to walk

**Tests to Run:**
- Vitamin D
- CRP/ESR (inflammation)
- CBC
- Body composition analysis

**Routes To:**
- MSK (Musculoskeletal) workflow
- Vitamin D supplementation
- Physiotherapy

---

### 4. **Cough / Cold / Sneezing**

**May Point To:**
- Viral URI (most common)
- Allergy/rhinitis
- Throat infection
- Asthma
- TB (if chronic - important in India)
- COPD (smokers)
- GERD

**Follow-up Questions:**
```
- "Kitne din se?" (How many days?)
- "Khaansi sukhi hai ya balgam?" (Dry or with phlegm?)
- "Balgam ka rang?" (Phlegm color?)
- "Khoon aaya?" (Blood in sputum?)
- "Bukhaar, saans phoolna?" (Fever, breathlessness?)
- "Smoking karte hain?" (Do you smoke?)
```

**⚠️ RED FLAGS → URGENT/TB SCREEN:**
- Cough >3 weeks
- Blood in sputum
- Weight loss + night sweats
- High fever
- Breathlessness

**Tests to Run:**
- Temperature
- SpO2 (oxygen level)
- Pulse
- Lung sounds (stethoscope)
- CRP (if fever)
- **TB screen** (if chronic + systemic)

**Routes To:**
- Physician referral
- TB screening pathway
- Respiratory workflow

---

### 5. **Fever**

**May Point To:**
- Viral infection (most common)
- Bacterial infection
- Malaria/Dengue/Typhoid (endemic in India)
- UTI
- TB

**Follow-up Questions:**
```
- "Kitne din se?" (How many days?)
- "Kapkapti/thandi lagti hai?" (Chills/rigors?)
- "Koi rash?" (Any rash?)
- "Pet dard, peshaab mein jalan?" (Abdominal pain, burning urination?)
- "Kahin travel kiya?" (Any travel?)
```

**⚠️ RED FLAGS → URGENT:**
- High fever >3 days
- Breathlessness
- Rash with fever
- Drowsiness/confusion
- Very low urine output

**Tests to Run:**
- Temperature
- Pulse, BP, SpO2
- CBC (infection pattern)
- CRP/ESR
- Malaria/dengue rapid test (endemic areas)

**Routes To:**
- Physician referral (immediate)
- Endemic fever pathway
- Hospitalization (if severe)

---

### 6. **Dizziness / Weakness**

**May Point To:**
- Anaemia
- Low blood pressure
- Low blood sugar (hypoglycemia)
- Dehydration
- Inner ear problem
- Anxiety
- Cardiac issues

**Follow-up Questions:**
```
- "Khade hone par hota hai?" (On standing?)
- "Behosh hue?" (Fainted?)
- "Dil ki dharkan tez?" (Palpitations?)
- "Khaana chhoda?" (Skipped meals?)
```

**⚠️ RED FLAGS → URGENT:**
- Actual fainting
- Chest pain
- Slurred speech
- One-sided weakness (stroke signs)

**Tests to Run:**
- Blood pressure (lying and standing)
- Blood sugar
- Haemoglobin
- ECG
- Pulse rhythm

**Routes To:**
- Anaemia workflow
- Metabolic workflow
- **Emergency referral** (if cardiac/neuro signs)

---

### 7. **Acidity / Digestion Issues**

**May Point To:**
- Gastritis/GERD
- Irregular meals
- Stress
- H. pylori infection
- Peptic ulcer

**Follow-up Questions:**
```
- "Khaane ke baad hota hai?" (After meals?)
- "Raat ko zyada?" (Worse at night?)
- "Wajan kam hua?" (Weight loss?)
- "Kaala peshab?" (Black stools?)
- "Bhook kaisi hai?" (Appetite?)
```

**⚠️ RED FLAGS → URGENT:**
- Black/tarry stools (bleeding)
- Vomiting blood
- Unintentional weight loss
- Difficulty swallowing

**Tests to Run:**
- Haemoglobin (if bleeding suspected)
- Symptom-based evaluation

**Routes To:**
- Physician referral
- Anaemia workflow (if bleeding signs)
- Nutrition counseling

---

## 🏥 Baseline Checks - FOR EVERYONE

These run **regardless of complaint**. Quick, catch silent disease, guarantee useful results.

| Check | Machine | Why Essential |
|-------|---------|---------------|
| **Blood Pressure** | Omron BP Monitor | Silent hypertension is common & symptomless |
| **Pulse & Rhythm** | Spandan ECG | Catches irregular rhythm, rate abnormalities |
| **Height, Weight, Waist** | Body Analyzer | BMI + central obesity = metabolic risk |
| **Random Blood Sugar** | Glucometer | Undiagnosed diabetes often silent |
| **SpO2** | Pulse Oximeter | Oxygen saturation baseline |
| **Temperature** | Contactless | Infection screening |

---

## 🧬 Optional Wellness Panel (Opt-in)

**General Wellness Panel** - High value screening:
- CBC (Complete Blood Count) - anaemia, infection
- TSH (Thyroid) - very common, often missed
- HbA1c (Diabetes) - 3-month sugar average
- Lipid Profile - heart health
- Kidney Function (Creatinine)
- Vitamin D - deficiency epidemic in India
- Liver Function (if indicated)

**Age/Sex Nudges:**
- **30+** → Sugar & BP yearly
- **Women** → Anaemia & Thyroid check
- **40+** → Lipid & Kidney profile
- **50+** → Comprehensive annual screen

---

## 🎯 Smart Routing Logic

```
COMPLAINT(S) INPUT
       ↓
FOLLOW-UP QUESTIONS (detailed exploration)
       ↓
RED FLAG CHECK (SNOOP, danger signs)
       ↓
   RED FLAG?
   ├─ YES → URGENT REFERRAL (stop routine)
   └─ NO  → Continue
       ↓
BASELINE CHECKS (everyone gets these)
       ↓
PATTERN RECOGNITION
       ↓
ROUTE TO WORKFLOW(S):
   ├─ Anaemia
   ├─ Thyroid
   ├─ Metabolic/Diabetes
   ├─ PCOS
   ├─ MSK (Musculoskeletal)
   ├─ Mental Health
   ├─ Respiratory
   ├─ Physician Referral
   └─ General Wellness (if nothing specific)
       ↓
AWIS SCORE (unified wellness metric)
       ↓
NEXT STEPS & FOLLOW-UPS
```

---

## 💡 Engagement Strategy

### Keep Patients Coming Back (Responsibly):

1. **Every visit gets concrete results** - baseline panel ensures no one leaves empty-handed

2. **AWIS Score** - Single number people want to improve, drives return visits

3. **Abnormal results = scheduled recheck** - Creates follow-up relationship

4. **Link to Wellness Services:**
   - Nutrition counseling
   - IV therapy
   - Physiotherapy
   - Dermatology
   - Mental health support

5. **Screening nudges** (age/sex appropriate):
   - "Aapki age mein thyroid check zaroori hai"
   - "Women mein anaemia common hai, check kar lete hain"

6. **Frame honestly** - "Ye screening hai, diagnosis nahi" (This is screening, not diagnosis)

### ⚠️ Guardrail:
- Only recommend tests **clinically justified** by symptoms, risk factors, or screening guidelines
- Dr. Dheer signs off on test-recommendation rules
- **Never over-test for engagement's sake**

---

## 📊 Implementation in AAHA

### Current Status:
✅ Basic chat with symptom collection  
✅ Test selection interface  
✅ Individual test consent  
✅ Mini result display  
✅ AWIS score calculation  
✅ PCOS workflow integrated  

### Needs Addition:
🔄 Red flag screening questions  
🔄 Complaint-specific follow-up trees  
🔄 Smart routing logic  
🔄 Baseline panel for everyone  
🔄 Wellness panel opt-in  
🔄 Age/sex screening nudges  
🔄 Follow-up scheduling  

---

## 🗣️ Conversational Examples

### Example 1: Headache + Tiredness

**AAHA**: "Aap batayein — kya taklif ho rahi hai?"  
**PATIENT**: "Mujhe sar dard aur thakaan mehsoos hoti hai."  

**AAHA**: "Pehle sar dard ki baat karte hain. Kab se ho raha hai?"  
**PATIENT**: "Kuch hafton se."  

**AAHA**: "Roz hota hai ya kabhi kabhi?"  
**PATIENT**: "Lagbhag roz."  

**AAHA**: "Kahan zyada mehsoos hota hai — maathe par ya peeche?"  
**PATIENT**: "Aankhon ke peeche aur maathe par."  

**AAHA**: "Kuch zaroori sawaal — ye dard achanak, sabse tez tarah se toh nahin aaya?" [RED FLAG CHECK]  
**PATIENT**: "Nahin."  

**AAHA**: "Ab thakaan ki baat. Thand zyada lagti hai, ya wajan mein badlaav?"  
**PATIENT**: "Thand zyada lagti hai, wajan badh gaya."  

**Pattern detected**: Headache (tension) + Tiredness + Cold intolerance + Weight gain  
**Route to**: Thyroid + Anaemia workflows  
**Tests**: TSH, Haemoglobin, BP, Sugar (baseline)

---

### Example 2: Cough >3 weeks

**AAHA**: "Khansi kitne din se hai?"  
**PATIENT**: "Karib ek mahina ho gaya."  

**AAHA**: "Balgam mein kabhi khoon aaya?" [RED FLAG]  
**PATIENT**: "Haan, ek do baar."  

**AAHA**: "Wajan kam hua, ya raat ko pasina aata hai?" [TB SCREENING]  
**PATIENT**: "Haan, wajan thoda kam hua."  

**Pattern detected**: Chronic cough + Blood in sputum + Weight loss  
**Action**: **TB screening pathway** + Physician referral (urgent)  
**Tests**: Sputum test, Chest X-ray, Temperature, CBC

---

## 🔐 Safety First - Red Flag Protocol

### SNOOP Criteria (Headache):
- **S**udden 'worst ever'
- **N**eurological signs
- **O**nset with exertion
- **O**lder age new-onset
- **P**attern change

### General Red Flags:
- Unintentional weight loss
- Night sweats
- Blood (in any form)
- Chest pain
- Breathlessness at rest
- Neurological symptoms
- High fever >3 days
- Severe pain

**When red flag detected:**
1. Stop routine workflow
2. Flag for immediate physician review
3. Provide reassurance to patient
4. Document clearly
5. Arrange urgent referral/transport if needed

---

## 📈 Success Metrics

### Clinical:
- % of silent hypertension detected
- % of undiagnosed diabetes caught
- % of anaemia identified
- % of thyroid disorders found
- Red flag referral accuracy

### Engagement:
- Return visit rate
- Wellness panel uptake
- Follow-up completion rate
- AWIS score improvement over time
- Patient satisfaction

### Safety:
- Zero missed red flags
- Appropriate referral rate
- Test recommendation appropriateness

---

## 🚀 Next Implementation Steps

1. **Build complaint library** with follow-up question trees
2. **Implement red flag screening** at conversation level
3. **Add smart routing engine** - pattern → workflow mapper
4. **Create baseline panel logic** - runs for everyone
5. **Add wellness panel opt-in** with age/sex nudges
6. **Build follow-up scheduler** for abnormal results
7. **Integrate with existing PCOS workflow**
8. **Test with real patient scenarios**
9. **Get Dr. Dheer sign-off** on clinical rules

---

**Status**: 📋 Documented, Ready for Implementation  
**Priority**: 🔴 High - This is the main entry point  
**Owner**: Development Team + Dr. Dheer (Clinical Validation)

