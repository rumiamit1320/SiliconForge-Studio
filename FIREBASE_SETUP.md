# SiliconForge Studio — Firebase setup

The HTML application uses Firebase Authentication (Email/Password) and Cloud Firestore. Browser `localStorage` and JSON backup remain available as a local fallback.

## One-time Firebase Console setup

1. Open the Firebase project **substationops-database**.
2. In **Build → Authentication → Sign-in method**, enable **Email/Password**.
3. In **Build → Firestore Database**, create a database. Choose the region appropriate for your users.
4. Open Firestore **Rules**, paste the contents of `firestore.rules` from this repository, and click **Publish**.
5. In **Authentication → Settings → Authorized domains**, confirm `siliconforge-studio.onrender.com` is listed. Add it if it is missing.
6. Open https://siliconforge-studio.onrender.com, choose **Create Account**, and register with an email address and password. Demo profiles remain local-only and do not authenticate to Firebase.

## Data model

- `users/{uid}`: profile metadata for the signed-in Firebase user.
- `users/{uid}/vault/main`: the user's SiliconForge database vault and current chip/ISA state.
- Firestore rules restrict each signed-in user to their own profile and vault.
- The browser keeps local copies in `localStorage`; cloud sync starts after Firebase sign-in.

## Important notes

- The Firebase web API key in an HTML app is not a server secret. Protect data with Authentication and Firestore Security Rules.
- The existing demo RBAC role selector is a UI/demo feature. It is not an authorization boundary for cloud data.
- This integration does not implement TOTP MFA. The pre-existing TOTP display is not a real second factor. Configure Firebase multi-factor authentication separately if required.
- Do not deploy permissive test rules to production.
