# KP WhatsApp — Secure KPAI Project

This project contains the existing KP WhatsApp frontend plus a Firebase Cloud Functions backend for KPAI.

## Security
- The Gemini API key is NOT included anywhere in the frontend or this project.
- Store it with Firebase Secret Manager.
- The function requires a Firebase Auth ID token.
- Rate limit: 20 requests per authenticated user per minute.

## Deploy
1. Install Firebase CLI: `npm install -g firebase-tools`
2. Login: `firebase login`
3. Copy `.firebaserc.example` to `.firebaserc` and set your Firebase project ID.
4. Run: `firebase use YOUR_PROJECT_ID`
5. Set the Gemini secret: `firebase functions:secrets:set GEMINI_API_KEY`
6. From this folder run: `cd functions && npm install && cd ..`
7. Deploy: `firebase deploy --only functions,hosting`

## Frontend endpoint
The Hosting rewrite exposes `/api/kpai` to the deployed function. The frontend must send:
`Authorization: Bearer <Firebase ID token>`
with JSON `{ "message": "..." }`.

## Important
The supplied frontend is the existing KP WhatsApp HTML. Its existing Firebase configuration/auth flow should be retained. Wire its KPAI send handler to `/api/kpai` using the current signed-in Firebase user's ID token.

## Model
The backend requests `gemini-3.5-flash-lite` as requested. If that model is not available in your Google AI account/API version, change the `model` constant in `functions/index.js` to an available Gemini model.
