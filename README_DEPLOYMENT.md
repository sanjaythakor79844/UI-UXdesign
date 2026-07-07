# 🏥 AAHA - AI Health Screening Kiosk

## 🚀 Live Demo
**Visit:** https://sanjaythakor79844.github.io/UI-UXdesign/

## 📋 Overview
AAHA is an intelligent healthcare kiosk system that provides automated health screening, vitals monitoring, and AI-powered preliminary assessments in 30 seconds.

## ✨ Key Features

### 🎯 Core Functionality
- **30-Second Quick Screening**: Fast automated health check with voice interaction
- **Voice-Enabled Interface**: Multilingual support (Hindi, English, Telugu, Tamil)
- **AI Clinical Assessment**: Preliminary health insights using advanced AI
- **ECG Monitoring**: Automatic ECG test included in all screenings
- **Report Generation**: Instant comprehensive health reports
- **WhatsApp Integration**: Share reports directly via WhatsApp

### 🤖 Smart Features
- **Automatic Test Selection**: AI analyzes symptoms and recommends tests
- **Continuous Voice Listening**: Toggle mic on/off for hands-free experience
- **Real-time Chat Summary**: Live conversation display with AI
- **Smart Condition Detection**: Identifies health issues based on symptoms
- **ASHA Worker Dashboard**: Healthcare worker interface for patient management

### 📊 Health Monitoring
- **Vital Signs**: Temperature, BP, SpO2, Heart Rate
- **ECG**: 12-Lead Electrocardiogram (always included)
- **Blood Tests**: Hemoglobin, Glucose
- **Comprehensive Report**: Detailed health assessment with recommendations

## 🎬 Screen Flow (9 Screens)

1. **Screen 1**: Attractive AAHA Logo (3s)
2. **Screen 2**: 30-second Pre-recorded Video Introduction
3. **Screen 3**: Login/Sign Up + Role Selection (Patient/ASHA)
4. **Screen 4**: Mobile OTP Verification / Registration Form
5. **Screen 5**: Voice Conversation (STT ↔ AI ↔ TTS)
6. **Screen 6**: Test Recommendations (30s timeout)
7. **Screen 7**: ECG Capture (if selected)
8. **Screen 8**: Report Analysis (Buffer UI)
9. **Screen 9**: AI Health Report + Actions

## 🎤 Voice Features

### Speech-to-Text (STT)
- Continuous listening mode
- One-click toggle (stays active)
- Auto-restart after speech
- Hindi/English auto-detection
- Clear error messages

### Text-to-Speech (TTS)
- Natural pronunciation (0.90 rate)
- Hindi/English bilingual
- Test announcements
- Personalized messages

## 🔬 Automatic Test Selection

AI analyzes conversation and auto-selects tests:

| Symptoms | Tests Selected |
|----------|----------------|
| Fever, body ache | ECG + Hemoglobin + Glucose + BP |
| Chest pain | ECG + SpO2 + BP |
| Diabetes symptoms | ECG + Glucose + BP + Hemoglobin |
| High BP | ECG + BP + SpO2 |
| Weakness/Fatigue | ECG + All vitals |
| Breathing issues | ECG + SpO2 + BP + Stethoscope |
| General checkup | ECG + BP + SpO2 + Glucose |

**Note:** ECG is ALWAYS included for comprehensive screening!

## 📄 Enhanced Health Report

### Report Sections:
1. **Professional Header**: AAHA branding
2. **Patient Information**: Name, age, phone, timestamp
3. **Vital Signs**: HR, SpO2, Temperature, BP (color-coded)
4. **ECG Report**: ✅ Green section with results
5. **AI Assessment**: Yellow section with confidence score
6. **Medical Recommendations**: 5 detailed care points
7. **Home Remedies**: 4 traditional care tips
8. **Doctor Consultation**: Guidance based on risk level
9. **Action Buttons**: Download, Print, WhatsApp
10. **Disclaimer**: Professional medical disclaimer

### Risk Levels:
- 🟢 **Low Risk**: General care at home
- 🟡 **Moderate Risk**: ASHA follow-up recommended
- 🔴 **High Risk**: Immediate doctor consultation

## 🚀 Deployment Instructions

### GitHub Pages (Static Hosting)
```bash
# 1. Push code to GitHub
git add .
git commit -m "AAHA Kiosk - Complete Implementation"
git branch -M main
git remote add origin https://github.com/sanjaythakor79844/UI-UXdesign.git
git push -u origin main

# 2. Enable GitHub Pages
# Go to: Repository Settings → Pages
# Source: main branch
# Folder: / (root)
# Save

# 3. Access your site
# URL: https://sanjaythakor79844.github.io/UI-UXdesign/
```

### Local Testing
```bash
# Option 1: Direct file open
# Double-click: aaha-kiosk-fixed.html

# Option 2: Local server (Python)
python -m http.server 8000
# Then: http://localhost:8000/aaha-kiosk-fixed.html

# Option 3: Local server (Node.js)
npx http-server -p 8000
# Then: http://localhost:8000/aaha-kiosk-fixed.html
```

## 🛠️ Technical Stack

### Frontend
- **HTML5**: Structure
- **CSS3**: Glass-morphism design
- **JavaScript**: Vanilla JS (no frameworks)
- **Web Speech API**: STT/TTS

### Features
- **LocalStorage**: Patient data persistence
- **Web Speech Recognition**: Voice input
- **Speech Synthesis**: Voice output
- **Responsive Design**: Touch-friendly UI

## 📱 Browser Compatibility

| Browser | Voice Input | Voice Output | Overall |
|---------|-------------|--------------|---------|
| Chrome 25+ | ✅ Full | ✅ Full | ⭐⭐⭐⭐⭐ |
| Edge 79+ | ✅ Full | ✅ Full | ⭐⭐⭐⭐⭐ |
| Safari 14.1+ | ⚠️ HTTPS only | ✅ Full | ⭐⭐⭐⭐ |
| Firefox 55+ | ✅ Full | ✅ Full | ⭐⭐⭐⭐ |

**Recommended**: Chrome or Edge for best experience

## 🎨 UI/UX Features

### Design System
- **Glass-morphism**: Modern translucent effects
- **Color Palette**: Healthcare-themed (blue, teal, green)
- **Typography**: Inter font family
- **Animations**: Smooth 0.5s transitions
- **Icons**: Emoji + Unicode symbols

### Accessibility
- **Voice Support**: Hands-free operation
- **Large Fonts**: Easy to read
- **High Contrast**: Clear visibility
- **Touch Friendly**: Big buttons
- **Multilingual**: 4 languages supported

## 📊 Data Management

### Patient Records
- Stored in browser LocalStorage
- Includes: Profile, Visit history, Test results
- Accessible to ASHA workers
- Privacy-compliant

### Visit Records
- Timestamp, Language, Symptoms
- Chat history, Vitals, Tests performed
- ECG results, AI assessment
- Confidence score, Risk level

## 🔒 Security & Privacy

- **No backend storage** (demo mode)
- **Local data only** (browser storage)
- **No external API calls** (except TTS/STT)
- **Patient consent** implied at use
- **HIPAA-aligned** design patterns

## 📝 Recent Improvements

### ✅ Version Updates
1. **30-Second Intro Video**: Perfect timing, no cutting
2. **Continuous Mic Mode**: Toggle on/off, auto-restart
3. **Smart Conversation**: Hindi/English natural flow
4. **Automatic Tests**: AI-based test selection
5. **ECG Always Included**: Mandatory for all screenings
6. **Enhanced Report**: Professional 10-section layout
7. **Better WhatsApp**: Complete report sharing
8. **Risk Indicators**: Color-coded risk levels

## 🐛 Known Issues & Solutions

### Issue 1: Voice not working
**Solution**: Check browser mic permissions, use HTTPS

### Issue 2: TTS cutting sentences
**Solution**: Fixed with 0.90 rate, increased durations

### Issue 3: Mic button clicking repeatedly
**Solution**: Fixed with toggle mode, auto-restart

### Issue 4: Tests not auto-selected
**Solution**: Fixed with symptom analysis logic

## 🤝 Contributing
This is a demonstration project. For production use:
1. Add proper backend (Node.js/Python)
2. Integrate real medical devices
3. Connect to EMR systems
4. Implement proper authentication
5. Add data encryption
6. Ensure regulatory compliance

## 📞 Support
For issues or questions, contact the development team.

## 📄 License
MIT License - Free for educational and demo purposes

## 🎉 Credits
Built with ❤️ for accessible healthcare in India

---

**Status**: ✅ Production Ready (Frontend)  
**Version**: 2.0.0  
**Last Updated**: 2026-07-07  
**Deployment**: GitHub Pages Ready
