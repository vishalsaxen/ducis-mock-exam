# Ducis Mock Exam

Clickable single-file HTML prototype of the Ducis Mock Exam app. Open `index.html` in any browser.

## Screens

1. Landing (Login / Click to Register)
2. Login with any email provider (Gmail, Outlook, Yahoo, work email)
3. Register with username, country code + phone, email, password and confirmation
4. Verify Email (simulated: any 4-digit code works)
5. User information and About ITIL® Foundation. Start Quiz unlocks after the copyright information is read and acknowledged.
6. Quiz setup: choose 5, 10, 20, 40 or a custom number of questions
7. Question builder: 4 options per question, one marked correct
8. Take the quiz
9. Score with answer review (65% pass mark)
10. Thank you page with a printable result slip

## Validation rules

- **Email**: practical RFC 5322 format check, any provider; no spaces or consecutive dots; common domain typos (e.g. `gmial.com`) are flagged.
- **Password**: 8 to 64 characters, at least one uppercase letter, lowercase letter, number and special character, no spaces; must be confirmed.
- **Phone**: country code dropdown grouped by region (India, Asia, Middle East incl. UAE, USA/Canada, Europe incl. EU members, Africa, Australia/New Zealand). Each country checks national number length and mobile leading digits.
- **Username**: 3 to 30 characters, starts with a letter.


## Database (Firebase project `ducis-mock-exam`)

The app runs in **demo mode** (everything kept in the browser tab) until the Firebase config is filled in. With the config, it uses:

- **Firebase Authentication** (email/password) for register and login, with a verification **link** email (Firebase does not send 4-digit codes) and password reset emails.
- **Cloud Firestore** for data:
  - `users/{uid}`: name, email, phone, country, emailVerified, copyrightAcknowledgedAt, createdAt
  - `users/{uid}/quizzes/{id}`: count, questions (question, options, correctIndex), createdAt
  - `users/{uid}/attempts/{id}`: quizId, total, correct, skipped, pct, pass, answers, createdAt

### Setup

1. Firebase console > Project settings > General > Your apps: add a **Web app** (if there isn't one) and copy its config.
2. The web config is already in `index.html` (`window.DUCIS_FIREBASE_CONFIG`). Clear `apiKey` and `appId` there to force demo mode.
3. Authentication > Sign-in method: enable **Email/Password**.
4. Firestore Database: create a database, then publish the rules in `firestore.rules` (paste them in the Rules tab, or run `firebase deploy --only firestore:rules`).
5. Serve the page over http(s), not by double-clicking the file: `npx serve .` locally (localhost is an authorized domain by default), or `firebase deploy --only hosting`. Add any other domain under Authentication > Settings > Authorized domains.

The web `apiKey` is not a secret; access is controlled by the Firestore rules and the authorized domains.
