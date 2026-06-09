# Face Attendance System

A web-based attendance management system using facial recognition and GPS location verification.

## Features

- **Admin Panel**: Manage faculty, students, and subjects
- **Faculty Dashboard**: Start attendance sessions, capture GPS location
- **Student Portal**: Register face, mark attendance with face + GPS verification
- **Real-time Validation**: Face matching (85%+ threshold) and GPS proximity (10m radius)
- **Firebase Integration**: Firestore database for storing all records

## Quick Start

1. Open `index.html` in a browser
2. Select your role: Admin, Faculty, or Student
3. Follow the demo flow in SETUP.md for a complete walkthrough

## Deployment

Deploy to Firebase Hosting:

```bash
npm install -g firebase-tools
firebase login
firebase deploy
```

## Tech Stack

- Frontend: Pure HTML/CSS/JS
- Database: Firebase Firestore
- Face Recognition: face-api.js (TinyFaceDetector model)
- Location: HTML5 Geolocation API

## Setup

See [SETUP.md](SETUP.md) for detailed Firebase configuration instructions.
