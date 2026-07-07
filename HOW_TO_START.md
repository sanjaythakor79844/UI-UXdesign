# How to Start the AAHA Kiosk Project

## ✅ Changes Made

### 30-Second Kiosk Brief Added
The kiosk now includes a comprehensive 30-second introduction on Screen 2 that explains:
- AAHA is an AI Health Assistant available 24/7
- Complete health screening in just 30 seconds
- Multilingual support (Hindi, English, Tamil, Telugu)
- Vital signs checking (temperature, BP, heart rate, SpO2)
- AI-powered health assessment
- ASHA worker and doctor support
- Voice-based interaction

## 🚀 How to Start the Kiosk

### Option 1: Direct Browser Open (Simplest)
1. Double-click the file: `aaha-kiosk-fixed.html`
2. It will open in your default web browser
3. The kiosk will start automatically with the splash screen
4. After 3 seconds, it will show the 30-second intro with AAHA speaking

### Option 2: Using a Local Web Server (Recommended for full features)

#### Using Python:
```bash
# If you have Python installed:
python -m http.server 8000
```

Then open your browser to: `http://localhost:8000/aaha-kiosk-fixed.html`

#### Using Node.js:
```bash
# If you have Node.js installed:
npx http-server -p 8000
```

Then open your browser to: `http://localhost:8000/aaha-kiosk-fixed.html`

## 🎯 What to Expect

1. **Splash Screen** (3 seconds)
   - Shows AAHA logo and branding
   
2. **Kiosk Brief** (30 seconds)
   - AAHA introduces itself
   - Explains capabilities in Hindi and English
   - Shows progress bar
   - You can skip using "Skip Intro →" button

3. **Role Selection**
   - Choose "I am a Patient" or "I am an ASHA Worker"
   
4. **Login/Sign Up**
   - Enter mobile number
   - Complete registration if new user

5. **Health Screening**
   - Voice-enabled symptom collection
   - Vital signs measurement
   - AI assessment
   - Report generation

## 🔊 Audio Requirements

- The kiosk uses browser text-to-speech (TTS)
- Make sure your browser allows audio
- If prompted, click "Allow" for audio permissions
- Adjust volume as needed

## 🌐 Browser Compatibility

Works best on:
- ✅ Chrome/Edge (recommended)
- ✅ Firefox
- ✅ Safari
- ⚠️ Some features may be limited without a backend server

## 📱 Multilingual Support

The 30-second brief includes:
- Hindi dialogues for local connectivity
- English subtitles for understanding
- Automatic language detection for voice input

## 🛠️ Troubleshooting

**If the offline banner appears:**
- ✅ **FIXED**: The offline banner no longer appears incorrectly
- The kiosk now properly detects connectivity when opened directly
- Network checks are skipped for file:// protocol (demo mode)
- Banner only shows if you're genuinely offline

**If audio doesn't play:**
- Check browser audio permissions
- Unmute your device
- Try Chrome/Edge browsers

**If the intro doesn't start:**
- Refresh the page (F5)
- Clear browser cache (Ctrl+Shift+Del)
- Check browser console for errors (F12)

**If you want to skip the intro:**
- Click the "Skip Intro →" button
- Or wait for the full 30 seconds

## 📝 Notes

- This is a standalone HTML file with embedded JavaScript
- No backend required for basic UI flow
- All data is stored in browser localStorage
- Perfect for demonstration and testing
- For production, connect to actual backend APIs

---

**Ready to test?** Just open `aaha-kiosk-fixed.html` in your browser! 🚀
