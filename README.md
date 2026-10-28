# FM House — Fayjane Madina Product Catalog Website

Responsive bilingual (Bangla/English) dark-theme product catalog website, designed to later share the same Firebase backend with an Android app named **FM House**.

## Features
- Responsive mobile and desktop website
- Bangla / English interface toggle
- Firebase email/password sign-in and account creation
- Product list, live Firestore updates, search and category filter
- Admin-only product add/edit/delete and image upload
- Firebase Authentication custom claim `admin` for secure admin role
- Firestore and Storage security rules included

## Important
This package is a deployable website source, not a live public URL yet. To create a real URL, you must connect it to your own Firebase project and deploy it to Firebase Hosting. Do not put service-account keys or admin credentials in browser code.

## Setup and deploy

### 1. Install tools
Install Node.js LTS and Firebase CLI:
```bash
npm install -g firebase-tools
firebase login
```

### 2. Create Firebase project
At https://console.firebase.google.com/
1. Create a project, e.g. `fm-house-catalog`.
2. Add a Web App and copy its Firebase config.
3. Enable **Authentication → Sign-in method → Email/Password**.
4. Create a **Cloud Firestore** database.
5. Enable **Storage**.
6. In Authentication → Settings, add your Hosting domain to authorized domains after deploy if needed.

### 3. Add web config
Open `index.html` and replace the placeholder values in `firebaseConfig`:
- `PASTE_API_KEY`
- `PASTE_PROJECT_ID`
- `PASTE_MESSAGING_SENDER_ID`
- `PASTE_APP_ID`
- Storage bucket should match the value shown in Firebase project settings. Modern projects may use a `.firebasestorage.app` bucket rather than `.appspot.com`.

Firebase web config is intended for client apps; security is enforced by the rules. Never add a service account key here.

### 4. Set up Firestore and Storage rules
In Firebase Console, publish the contents of:
- `firestore.rules`
- `storage.rules`

Rules deliberately require sign-in for catalog reads and the `admin: true` custom claim for changes. Do not change the rules to public writes.

### 5. Make Alvi an admin securely
A user must first register in the website. Then set the Firebase Authentication custom claim using a trusted server environment (never in the website or app client).

Example Node.js script (run locally in a private admin-only environment):
```js
const admin = require('firebase-admin');
admin.initializeApp({
  credential: admin.credential.applicationDefault(),
});
async function main() {
  const email = process.argv[2];
  if (!email) throw new Error('Pass the registered admin email');
  const user = await admin.auth().getUserByEmail(email);
  await admin.auth().setCustomUserClaims(user.uid, { admin: true });
  console.log('Admin claim set for', email, user.uid);
}
main().catch(e => { console.error(e); process.exit(1); });
```
To run this, install `firebase-admin` in a separate secure folder, authenticate with Application Default Credentials using a trusted Google Cloud account, and run `node set-admin.js alvi@example.com`. Do not include credentials in the website bundle. Ask a developer if unsure.

After the custom claim is set, sign out and sign back in. The admin panel buttons should appear. Customer accounts do not get the admin claim.

### 6. Deploy to Firebase Hosting
From this folder:
```bash
firebase login
firebase init hosting
```
Choose the Firebase project; set public directory to `.`; configure as a single-page app: **No**; do not overwrite `index.html`.
Then:
```bash
firebase deploy --only hosting
```
Firebase will print a public `https://...web.app` URL. Share that URL with customers. Only signed-in users can read the catalog under the provided rules.

### 7. Seed categories
After Alvi is set as admin, use **Manage categories** to create categories. Then add products and images.

## Later Android app
The Android app named **FM House** can use the same Firebase project and data collections (`products`, `categories`). This avoids separate product lists and means a price update syncs across web and Android.

## Known starter limitations
- This is a starter implementation; test carefully before business use.
- Account creation is open by default. If you need customers to be approved by Alvi before seeing prices, implement an approval workflow and update the rules.
- Product images currently upload under a timestamped path. Deleting a product removes the Firestore record but does not automatically remove its Storage image; clean up unused images periodically.
- Currency display uses BDT.
