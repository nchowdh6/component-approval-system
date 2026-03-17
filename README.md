# Tier 2 Component Approval System

A web-based application for managing product/component approval workflows. Suppliers can register, submit component requests, and communicate with administrators who review and approve or reject submissions.

## Features

- **User Authentication** – Email/password registration and login via Firebase Auth
- **Product Submission** – Suppliers submit component requests with name, description, and company details
- **Admin Review** – Administrators can approve, reject, or request more information
- **Status Tracking** – Products move through Submitted → Approved / Rejected / Needs More Info
- **Communication** – Two-way comment thread on each product request

## Getting Started

### Prerequisites

- A [Firebase](https://firebase.google.com/) project with **Authentication** (Email/Password) and **Firestore Database** enabled

### Setup

1. Create a Firebase project at [console.firebase.google.com](https://console.firebase.google.com/)
2. Enable **Email/Password** authentication in Firebase Console → Authentication → Sign-in method
3. Create a **Firestore Database** in Firebase Console → Firestore Database
4. Copy your Firebase config from Project Settings → General → Your apps → Firebase SDK snippet
5. Replace the placeholder values in `index.html`:

   ```js
   const firebaseConfig = {
       apiKey: "YOUR_API_KEY",
       authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
       projectId: "YOUR_PROJECT_ID",
       storageBucket: "YOUR_PROJECT_ID.appspot.com",
       messagingSenderId: "YOUR_SENDER_ID",
       appId: "YOUR_APP_ID"
   };
   ```

6. Configure the admin email address by updating the `ADMIN_EMAILS` array in `index.html`:

   ```js
   const ADMIN_EMAILS = ['your-admin@example.com'];
   ```

7. Deploy the Firestore security rules (see [Firestore Security Rules](#firestore-security-rules) below)
8. Open `index.html` in a browser or deploy to Firebase Hosting

### Firestore Security Rules

Apply these rules in Firebase Console → Firestore Database → Rules:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    match /products/{productId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update: if request.auth != null;
    }
  }
}
```

> **Note:** For production use, tighten these rules with Firebase Custom Claims for admin role enforcement.

## Architecture

The application is a single-page app (`index.html`) using:

- **Frontend**: HTML5, CSS3, Vanilla JavaScript
- **Backend**: Firebase (Authentication + Firestore)
- **CDN**: Firebase SDK v9.6.0 (compat mode)

## Admin Access

Admin users are configured via the `ADMIN_EMAILS` array in `index.html`. Admin capabilities include:

- View all submitted products
- Approve or reject product requests
- Request more information from suppliers
- Communicate with suppliers via product comments
