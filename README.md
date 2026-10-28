# FM House — Fixed website files

## Included
- `index.html`: customer catalog is public; the three listed email accounts get Admin controls after login.
- `firestore.rules`: public product/category reads; only the three email accounts can write.
- `storage.rules`: public image reads; only the three email accounts can upload/manage product images (if Firebase Storage is available for this project).
- `firebase.json`: Firebase CLI rules configuration.

## Publish the website using GitHub Pages (phone-friendly)
1. Download and extract this ZIP.
2. Open your GitHub repository: https://github.com/afidrana/fm-house
3. Open `index.html` -> pencil/Edit -> replace all old code with the new `index.html` contents -> Commit changes.
4. Confirm the live site at https://afidrana.github.io/fm-house/

## Publish Firestore rules
Firebase Console -> Firestore Database -> Rules. Replace the rules with the contents of `firestore.rules`, then Publish.

## Storage rules
Firebase Console -> Storage -> Rules. Replace with `storage.rules`, then Publish. If Firebase asks to upgrade billing or says Storage is unavailable, do not add payment details unless you choose to; image uploads will need a different image-hosting option. Public image URLs can be used instead only after the website code is adjusted for that workflow.

## Admin emails
- samiulislam88598@gmail.com
- afid5354@gmail.com
- tasnimmahi88598@gmail.com

These emails are allowed by the website UI and by the Firestore/Storage rules. Confirm these are exactly the three intended Admin accounts before publishing. Anyone else can browse products and prices but cannot add/edit/delete data through Firebase rules.

## Important
The Firebase web configuration in `index.html` identifies the project and is intended to be used in a browser; access control is enforced by Firebase Rules. Do not share account passwords. Customers do not need to register or log in to see products.
