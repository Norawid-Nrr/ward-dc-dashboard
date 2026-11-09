# Ward D/C Dashboard (9/2)

Static dashboard hosted on GitHub Pages, using Firebase Authentication (Anonymous) and Cloud Firestore.

## Data collections

- `dc_cases`: discharge records (read/create/update/delete for anonymous-authenticated visitors).
- `settings/main`: dashboard target setting.

## Discharge history and pagination

- Browse discharge records by month or year (the year selector is displayed in Buddhist Era / พ.ศ.).
- The table is paginated, with 10, 25, 50, or 100 rows per page.
- Use first, previous, numbered, next, and last-page controls. Changing the month or year returns to page 1.
- Add, edit, and delete actions remain available from the table when connected to Firebase.
- No `audit_logs` collection is used or required.

## Important access warning

This project is configured for **no visible login**. The browser signs in anonymously in the background. Anyone who can open the public website can read, add, edit, and delete every document in `dc_cases`, and can change the dashboard target. Do not use this public configuration for identifiable patient information, HN, names, or confidential clinical details. The form is designed to use bed number only, but the public database is still not access-restricted.

## 1. Create Firebase project

1. Create a project at https://console.firebase.google.com/.
2. Add a Web App and copy its Firebase config.
3. In **Authentication → Sign-in method**, enable **Anonymous**.
4. In **Firestore Database**, create a database.
5. In Firestore **Rules**, publish the contents of `firestore.rules` (or deploy them using the Firebase CLI below).
6. Replace the four placeholders in `index.html` in `FIREBASE_CONFIG` with the values from Project settings → Your apps:
   - `apiKey`
   - `authDomain`
   - `projectId`
   - `appId`

The Firebase web config is intentionally in the client-side page. It is not a password; Firestore Security Rules control access.

## 2. Publish Firestore rules (CLI option)

Install Node.js, then run:

```bash
npm install -g firebase-tools
firebase login
cp .firebaserc.example .firebaserc
# Edit .firebaserc and replace YOUR_FIREBASE_PROJECT_ID
firebase use YOUR_FIREBASE_PROJECT_ID
firebase deploy --only firestore:rules
```

You can also paste `firestore.rules` into Firebase Console → Firestore Database → Rules and click Publish.

## 3. Push to GitHub

Create an empty GitHub repository, then from this folder run:

```bash
git init
git add .
git commit -m "Initial ward D/C dashboard"
git branch -M main
git remote add origin https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPOSITORY.git
git push -u origin main
```

## 4. Enable GitHub Pages

In the repository on GitHub:

1. Open **Settings → Pages**.
2. Under **Build and deployment**, choose **Deploy from a branch**.
3. Select branch `main` and folder `/ (root)`, then Save.
4. Wait for GitHub Pages to publish. The site URL will be shown on the Pages settings screen.

## 5. Test

1. Open the published URL in a private/incognito browser window.
2. Confirm the dashboard shows live Firebase status, not Demo mode.
3. Add a test record, refresh, edit it, and delete it.
4. Confirm the new record appears in the selected month/year history table. Test month and year filters, then test editing and deleting a record.

If it falls back to Demo mode, check the Firebase config, Anonymous sign-in setting, Firestore database, and published rules.


## Historical records
The dashboard history table reads directly from `dc_cases`. Select **รายเดือน** or **รายปี**, then choose a Buddhist Era year and (for monthly view) a month. No separate audit log collection is used.
