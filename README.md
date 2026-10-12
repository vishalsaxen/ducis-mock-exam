# Ducis Mock Exam

Clickable single-file HTML prototype of the Ducis Mock Exam app. Open `index.html` in any browser.

## Roles

- **Admin** creates mock exams: title, number of questions, 4 options per question with one marked correct. Exams can be saved as drafts, published, unpublished, edited and deleted. Duplicate options in a question (e.g. A = "AGRA" and D = "agra", ignoring case and extra spaces) are flagged as a conflict and block saving.
- **Student** reads the ITIL Foundation intro, acknowledges the copyright notice, picks a published exam, attempts it, sees the score with answer review and prints a result slip. Students cannot create or change questions.

## Screens

1. Landing (Login / Click to Register)
2. Login with any email provider (Gmail, Outlook, Yahoo, work email)
3. Register with username, country code + phone, email, password and confirmation
4. Verify Email (demo: any 4-digit code; Firebase: verification link)
5. Student home: user info and About ITIL® Foundation. Start Quiz unlocks after the copyright information is read and acknowledged.
6. Student: choose a published mock exam
7. Take the exam, then score with answer review (65% pass mark)
8. Thank you page with a printable result slip
9. Admin: dashboard of mock exams, exam details (title + question count) and question builder

## Validation rules

- **Email**: practical RFC 5322 format check, any provider; no spaces or consecutive dots; common domain typos (e.g. `gmial.com`) are flagged.
- **Password**: 8 to 64 characters, at least one uppercase letter, lowercase letter, number and special character, no spaces; must be confirmed.
- **Phone**: country code dropdown grouped by region (India, Asia, Middle East incl. UAE, USA/Canada, Europe incl. EU members, Africa, Australia/New Zealand). Each country checks national number length and mobile leading digits.
- **Username**: 3 to 30 characters, starts with a letter.


## Database (Firebase project `ducis-mock-exam`)

The app runs in **demo mode** (everything kept in the browser tab) until the Firebase config is filled in. With the config, it uses:

- **Firebase Authentication** (email/password) for register and login, with a verification **link** email (Firebase does not send 4-digit codes) and password reset emails.
- **Cloud Firestore** for data:
  - `users/{uid}`: name, email, phone, country, role (`student` or `admin`), emailVerified, copyrightAcknowledgedAt, createdAt
  - `exams/{id}`: title, published, count, questions (question, options, correctIndex), createdBy, createdAt, updatedAt
  - `users/{uid}/attempts/{id}`: examId, examTitle, total, correct, skipped, pct, pass, answers, createdAt

### Setup

1. Firebase console > Project settings > General > Your apps: add a **Web app** (if there isn't one) and copy its config.
2. The web config is already in `index.html` (`window.DUCIS_FIREBASE_CONFIG`). Clear `apiKey` and `appId` there to force demo mode.
3. Authentication > Sign-in method: enable **Email/Password**.
4. Firestore Database: create a database, then publish the rules in `firestore.rules` (paste them in the Rules tab, or run `firebase deploy --only firestore:rules`).
5. Serve the page over http(s), not by double-clicking the file: `npx serve .` locally (localhost is an authorized domain by default), or `firebase deploy --only hosting`. Add any other domain under Authentication > Settings > Authorized domains.

6. Make yourself an admin: register in the app, then in Firestore > Data open `users` > your document and change `role` from `student` to `admin`. Log out and back in.

In demo mode, the Login and Register forms have a "sign in as Student / Admin" switch instead.

Note: students' browsers receive the answer key with the exam so the score can be calculated in the page. Moving grading to a Cloud Function would hide it.

The web `apiKey` is not a secret; access is controlled by the Firestore rules and the authorized domains.
