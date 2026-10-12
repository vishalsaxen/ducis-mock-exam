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

Auth, email codes and storage are simulated in the browser; there is no backend yet.
