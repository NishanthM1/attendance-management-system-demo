# Face Attendance System - Setup Guide

## Firebase Setup (Required)

### 1. Create Firebase Project
1. Go to https://console.firebase.google.com/
2. Click "Add project" 
3. Name it: `attendance-demo-demo` (or any name you prefer)
4. Disable Google Analytics (not needed for demo)
5. Click "Create project"

### 2. Create Firestore Database
1. In Firebase Console, go to "Build" → "Firestore Database"
2. Click "Create database"
3. Select **"Start in test mode"** (for demo purposes)
4. Choose a location close to you (e.g., `asia-south1` for India)

### 3. Update Firebase Config
The app uses this Firebase config (already configured):
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyDqJZTE8r8r3S1NUXD77S1NRHBbbvXKDljLo",
  authDomain: "attendance-demo-demo.firebaseapp.com",
  projectId: "attendance-demo-demo",
  storageBucket: "attendance-demo-demo.firebasestorage.app",
  messagingSenderId: "63825400742",
  appId: "1:63825400742:web:2f2fd96662f5b2f6b37567",
  measurementId: "G-B8CWL13PT6"
};
```

If you create a NEW Firebase project, update this config in all HTML files with your project's config.

### 4. Firestore Security Rules (Important!)
In Firebase Console → Firestore → Rules tab, set:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /students/{studentId} { allow read, write: if true; }
    match /faculty/{facultyId} { allow read, write: if true; }
    match /subjects/{subjectId} { allow read, write: if true; }
    match /attendanceSessions/{sessionId} { allow read, write: if true; }
    match /attendance/{recordId} { allow read, write: if true; }
    match /attendanceRecords/{recordId} { allow read, write: if true; }
  }
}
```

Click "Publish" to save rules.

### 5. Run the App
Open `index.html` in a browser:
- **Option 1:** Double-click `index.html`
- **Option 2:** Use a local server:
  ```bash
  # Using Python
  python -m http.server 8080
  
  # Using Node.js
  npx serve .
  
  # Using VS Code Live Server extension
  # Right-click index.html → "Open with Live Server"
  ```
Then open http://localhost:8080 in your browser.

---

## Complete Demo Flow (Viva Presentation)

### Admin Workflow
1. Open `index.html` → Click **"👨‍💼 Admin"**
2. **Add Faculty**:
   - Name: `Dr Kumar`
   - Email: `kumar@gmail.com`
   - Password: `123456`
   - Department: `Computer Science`
3. **Add Student**:
   - Name: `Nishanth`
   - Email: `nishanth@gmail.com`
   - Password: `123456`
   - Class: `MCA`
4. **Create Subject**:
   - Subject Name: `Cloud Computing`
   - Subject Code: `CC501`
   - Faculty: `Dr Kumar`
5. **Assign Students** to Subject:
   - Select `Cloud Computing`
   - Check `Nishanth`
   - Click "Assign Students"

### Faculty Workflow
1. Click **"👩‍🏫 Faculty"** or go to `faculty.html`
2. Login with: `kumar@gmail.com` / `123456`
3. In Dashboard, click **"Start Attendance"** for Cloud Computing
4. System captures faculty GPS location and starts session

### Student Workflow
1. Click **"👨‍🎓 Student"** and login with: `nishanth@gmail.com` / `123456`
2. **Register Face** (first time only):
   - Click **"Open Camera"**
   - Click **"Capture & Register Face"**
3. **Mark Attendance**:
   - Select `Cloud Computing` (if active session)
   - Click **"Open Camera"**
   - Click **"Mark Attendance"**
   - System verifies face + GPS distance

### System Verification
1. Face Verification: Compares current face with registered face (85%+ match required)
2. GPS Verification: Student must be within 10 meters of faculty
3. Duplicate Check: Prevents multiple attendance for same subject on same day
4. Records stored in Firebase Firestore

### Reports
- **Admin**: Go to `report.html` to see all attendance records
- **Faculty**: Go to `faculty_dashboard.html` → "View Attendance" to see subject records

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "Camera access denied" | Allow camera permission in browser |
| "Face models not loading" | Check internet connection (models load from CDN) |
| "GPS not working" | Use HTTPS or localhost (GPS requires secure context) |
| "Firestore permission denied" | Check Firestore rules in Firebase Console |
| Face not detecting | Ensure good lighting, face is centered in frame |

## Tech Stack
- **Frontend**: Pure HTML/CSS/JS (no framework needed)
- **Database**: Firebase Firestore
- **Face Recognition**: face-api.js (TinyFaceDetector model)
- **Location**: HTML5 Geolocation API

## Notes
- Password stored in plain text (acceptable for demo/MCA project - mention this in viva)
- Test mode security rules allow all reads/writes (change for production)
- Face matching threshold: 0.6 (85%+ match required)
- Duplicate attendance prevented by checking same student + same date
