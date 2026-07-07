# 🎯 AAHA Kiosk - Complete Screen Flow

## ✅ Perfect Flow According to Requirements

### 📱 Screen 1: Attractive AAHA Logo
**Screen ID:** `screen-splash`  
**Duration:** 3 seconds  
**Purpose:** Brand introduction with attractive animated logo

**Features:**
- ✨ Animated orb with glowing eyes
- 🎨 Large AAHA branding text
- 💫 Smooth animations and effects
- 🎭 Professional healthcare kiosk appearance

**Auto-advances to:** Screen 2 after 3 seconds

---

### 🎬 Screen 2: Pre-recorded AAHA Video
**Screen ID:** `screen-video`  
**Duration:** 5-10 seconds (currently 8 seconds)  
**Purpose:** Quick introduction to AAHA's capabilities

**Content:**
1. **"Namaste! Main AAHA hoon — AI Health Assistant"**
   - Subtitle: Your 24/7 health companion at PHC
   
2. **"Sirf 30 seconds mein complete health screening"**
   - Subtitle: Fast, accurate, AI-powered assessment
   
3. **"Chaliye shuru karte hain aapki sehat ki jaanch"**
   - Subtitle: Let's begin your health screening journey

**Features:**
- 🎙️ Text-to-Speech (TTS) narration
- 📊 Progress bar showing time remaining
- ⏭️ Skip button available anytime
- 🌐 Bilingual (Hindi + English subtitles)

**Auto-advances to:** Screen 3 after completion

---

### 🎭 Screen 3: Login/Sign Up Page
**Screen ID:** `screen-role`  
**Purpose:** User role selection and authentication mode

**Two Options Given:**
1. **🧑‍🦱 I am a Patient**
   - Get screened & see your own reports
   
2. **🩺 I am an ASHA Worker**
   - View patient records & history

**Authentication Mode:**
- 🔐 **Login** button (for existing users)
- ✍️ **Sign Up** button (for new users)

**Flow:**
- Select role → Enables auth buttons → Select Login/Sign Up → Advances to Screen 4

---

### 📱 Screen 4A: LOGIN - Mobile Number & OTP
**Screen ID:** `screen-otp`  
**Purpose:** Verify existing user via mobile OTP

**Features:**
- 📞 10-digit mobile number input
- ⌨️ On-screen numeric keypad
- 🔢 OTP verification (simulated)
- ✅ Automatic validation

**Process:**
1. Enter 10-digit mobile number
2. Click "Verify OTP"
3. System checks if account exists
4. If exists → Login successful → Screen 5
5. If not exists → Redirect to Sign Up

---

### ✍️ Screen 4B: SIGN UP - Registration Form
**Screen ID:** `screen-registration`  
**Purpose:** Create new patient profile

**Required Fields:**
- **First Name** (e.g., Sunita)
- **Last Name** (e.g., Devi)
- **Age** (e.g., 42)
- **Phone Number** (auto-filled from previous screen)

**Process:**
1. Fill all required fields
2. Click "Continue →"
3. Profile saved to local database
4. Brief success message
5. Auto-redirect to Login screen

---

### 💬 Screen 5: Interaction UI (STT ↔ TTS)
**Screen ID:** `screen-chat`  
**Purpose:** Voice-enabled symptom collection with AI

**Flow:**
```
Patient (STT) → Buffer (Thinking dots) → AI Response (TTS)
     ↓                                        ↓
Voice Input                            Voice Output
     ↓                                        ↓
Live Chat Summary displayed on UI
```

**Features:**
- 🎤 **STT (Speech-to-Text)**: Patient speaks symptoms
- 💭 **Buffer UI**: Shows "thinking dots" while processing
- 🔊 **TTS (Text-to-Speech)**: AI responds with voice
- 📝 **Live Chat Summary**: Full conversation visible on screen
- 🎯 **Suggested Responses**: Quick-reply chips for common answers
- 🌐 **Multilingual**: Hindi, English, Tamil, Telugu

**AI Behavior:**
- Asks focused follow-up questions (duration, severity, etc.)
- After 3-4 exchanges, suggests vital sign checks
- Detects when to move to test recommendations

**Advances to:** Screen 6 when AI mentions "vitals" or "medical devices"

---

### 🧪 Screen 6: AI Test Recommendations
**Screen ID:** `screen-devices`  
**Purpose:** Display recommended tests with details and pricing

**Display Information:**
- 🔬 **Test Name** (e.g., ECG, Blood Pressure)
- 🖥️ **Machine Name** (e.g., ECG Monitor, BP Monitor)
- 💰 **Cost** (e.g., ₹50, ₹20)
- ✅ **Selection Status** (Selected / Tap to select)
- 🔌 **Connection Status** (when applicable)

**Available Tests:**
| Test | Icon | Machine | Cost |
|------|------|---------|------|
| ECG | ❤️ | ECG Monitor | ₹50 |
| Blood Pressure | 🩸 | BP Monitor | ₹20 |
| Oxygen Saturation | 🫁 | Pulse Oximeter | ₹20 |
| Blood Glucose | 💉 | Glucometer | ₹40 |
| Hemoglobin | 🧪 | Hemoglobin Analyzer | ₹60 |
| Chest Sounds | 🩺 | Digital Stethoscope | ₹30 |

**⏱️ 30-Second Timer:**
- ⏰ Countdown displayed: "⏱ 30s to choose"
- ❌ If no selection → **Return to main screen**
- ✅ If selected → **Continue to Screen 6B/7**
- 🚫 Cancel button available anytime

**User Actions:**
- Click tests to select/deselect
- See total cost update in real-time
- Click "Start Tests" to proceed
- Click "Cancel" to return home

---

### 📈 Screen 6B: ECG Capture (Optional)
**Screen ID:** `screen-ecg`  
**Purpose:** If ECG test selected, capture real-time data

**Features:**
- 📊 Live ECG waveform animation
- ❤️ Real-time heart rate display (BPM)
- 🔴 Recording indicator (blinking red dot)
- 📶 Signal quality indicator
- ✅ "Done" button when complete

**Duration:** ~10-15 seconds of data capture

---

### ⚙️ Screen 7: Analyze Report (Buffer)
**Screen ID:** `screen-processing`  
**Purpose:** Show buffer UI while AI analyzes data

**Buffer Elements:**
- 🧠 Animated brain/processing icon
- 💫 Rotating AI orb
- 📊 Progress bar (0% → 100%)
- 📝 Step-by-step status text:
  - "Analyzing Symptoms..."
  - "Processing Device Data..."
  - "Cross-checking Vitals..."
  - "Generating Medical Report..."

**Duration:** ~4-5 seconds (simulated processing time)

**Auto-advances to:** Screen 8 when analysis complete

---

### 📄 Screen 8: AI Recommendation on Report
**Screen ID:** `screen-diagnosis`  
**Purpose:** Display comprehensive health report with AI recommendations

**Report Sections:**

#### 📊 **Vitals Summary**
- Confidence Score (78-97%)
- Heart Rate (BPM)
- SpO2 (%)

#### 🏥 **Assessment**
- Likely condition (e.g., "Common Viral Fever")
- Risk Level badge (Low/Medium/High)
- Confidence indicator

#### 💊 **Recommendations**
- Rest and hydration advice
- Monitoring instructions
- Return conditions (when to come back)

#### 🏡 **Home Remedies**
- Traditional remedies (e.g., turmeric milk)
- Self-care tips

#### 👨‍⚕️ **Doctor Consultation**
- Urgency level
- ASHA worker follow-up recommended
- Option to book doctor appointment

**Actions Available:**
- ⬇️ **Download PDF** - Save report locally
- 🖨️ **Print** - Print physical copy
- 📲 **Send WhatsApp** - Share report via WhatsApp
- 🏠 **Finish & Return Home** - End session

**Backend Integration:**
- 📱 Report automatically shared via WhatsApp
- 💾 Visit record saved to patient profile
- 🚩 Low-confidence cases flagged for ASHA review

---

### 👋 Screen 9: End of Conversation
**Screen ID:** `screen-end`  
**Purpose:** Gracefully end session and greet patient

**Features:**
- ✨ Animated farewell orb
- 💬 Personalized farewell message:
  - "Thank you, [Patient Name]"
  - "Your report has been sent to your WhatsApp"
  - "Take care, and stay healthy. Namaste."
- 🔊 TTS speaks farewell message
- ⏱️ Auto-return to home screen after 6 seconds

**Session Cleanup:**
- Clears all patient data
- Resets state variables
- Returns to Screen 1 (ready for next patient)

---

## 🔄 Complete Flow Diagram

```
┌─────────────────────────────────────────────────────────┐
│ Screen 1: AAHA Logo (3s)                                │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│ Screen 2: Pre-recorded Video (5-10s)                    │
│ [Skip button available]                                 │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│ Screen 3: Role Selection                                │
│ [Patient] or [ASHA Worker] + [Login/Sign Up]           │
└────────────────┬────────────────────────────────────────┘
                 │
         ┌───────┴───────┐
         │               │
         ▼               ▼
    ┌─────────┐    ┌──────────┐
    │ Login   │    │ Sign Up  │
    │ (4A)    │    │ (4B)     │
    └────┬────┘    └────┬─────┘
         │              │
         └──────┬───────┘
                │
                ▼
┌─────────────────────────────────────────────────────────┐
│ Screen 5: Interaction UI (STT ↔ AI ↔ TTS)              │
│ Voice conversation with live chat summary              │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│ Screen 6: Test Recommendations (30s timer)              │
│ [Select tests] or [Cancel] → Home                       │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
         ┌───────┴───────┐
         │               │
         ▼               ▼
    ┌─────────┐    ┌──────────┐
    │ Cancel  │    │ Selected │
    │ → Home  │    │ Tests    │
    └─────────┘    └────┬─────┘
                        │
                        ▼
               ┌─────────────────┐
               │ Screen 6B: ECG  │
               │ (if selected)   │
               └────────┬─────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────┐
│ Screen 7: Analyze Report (Buffer UI)                    │
│ Processing... 0% → 100%                                 │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│ Screen 8: AI Recommendation Report                      │
│ [Download] [Print] [WhatsApp] [Finish]                 │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│ Screen 9: End of Conversation                           │
│ Farewell message + Auto-return home (6s)               │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
            [Return to Screen 1]
```

---

## ✅ Requirements Checklist

| # | Requirement | Status | Screen |
|---|-------------|--------|--------|
| 1 | Attractive AAHA logo | ✅ Done | Screen 1 |
| 2 | 5-10 sec pre-recorded video | ✅ Done (8s) | Screen 2 |
| 3 | Login/Sign Up with 2 options | ✅ Done | Screen 3 |
| 3a | I am ASHA worker option | ✅ Done | Screen 3 |
| 3b | I am patient option | ✅ Done | Screen 3 |
| 4 | Login: Mobile + OTP | ✅ Done | Screen 4A |
| 4 | Sign Up: Name, Age, Phone | ✅ Done | Screen 4B |
| 5 | STT → Buffer → TTS | ✅ Done | Screen 5 |
| 5 | Live chat summary on UI | ✅ Done | Screen 5 |
| 6 | AI test recommendations | ✅ Done | Screen 6 |
| 6 | Test details, machine, cost | ✅ Done | Screen 6 |
| 6 | 30sec wait for selection | ✅ Done | Screen 6 |
| 6 | Return to home if no selection | ✅ Done | Screen 6 |
| 6 | User can cancel | ✅ Done | Screen 6 |
| 7 | Analyze report with buffer | ✅ Done | Screen 7 |
| 8 | AI recommendation on report | ✅ Done | Screen 8 |
| 8 | May ask for doc appointment | ✅ Done | Screen 8 |
| 8 | Share report via WhatsApp | ✅ Done | Screen 8 |
| 9 | End conversation & greet | ✅ Done | Screen 9 |

---

## 🎨 UI/UX Features

### Visual Design
- ✨ Glass-morphism effects
- 💫 Smooth animations and transitions
- 🎨 Healthcare-themed color palette
- 🌈 Gradient backgrounds
- 📱 Responsive and touch-friendly

### Accessibility
- 🔊 Voice output (TTS) for all interactions
- 🎤 Voice input (STT) support
- 🌐 Multilingual support (4 languages)
- 👁️ Large, readable fonts
- 🎯 High contrast elements

### User Experience
- ⚡ Fast transitions (0.5s max)
- 🔄 Auto-progression where logical
- ⏭️ Skip/Cancel options available
- 📊 Visual feedback for all actions
- ⏱️ Clear timing indicators

---

## 🚀 Current Status

### ✅ Fully Implemented
- All 9 screens properly labeled
- Complete flow from logo to farewell
- 30-second timeout on test selection
- WhatsApp report sharing
- ASHA worker dashboard
- Offline data storage

### 🎯 Ready for Testing
- Open `aaha-kiosk-fixed.html` in browser
- Experience complete patient journey
- Test all screen transitions
- Verify timeout behaviors
- Check role-based flows

---

*Last Updated: 2026-07-07*  
*Status: ✅ Perfect Flow Implemented*  
*Next: Backend Integration & Real Device Testing*
