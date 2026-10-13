# StudyFlow — Google Login connected to studyflow-6b7b6

The Firebase web app configuration has already been added to `index.html`. The site uses Firebase JavaScript SDK 13.0.0 directly from the Firebase CDN. No npm is needed to *run* the website.

## Activate Google login (required)

1. Open https://console.firebase.google.com/project/studyflow-6b7b6/authentication/providers and open **Google** under Sign-in method. Enable the Google provider, select a Project support email, and Save. If not already started, select Get started first.
2. Open https://console.firebase.google.com/project/studyflow-6b7b6/firestore and create the **Cloud Firestore** database if it is not already there. Select a region. Do not use public/test-mode rules on a live site.
3. Under **Firestore Database → Rules**, copy the contents of `firestore.rules` and Publish. These rules restrict reads/writes to `studyflowUsers/{signed-in-uid}`.
4. **Host** the website via an HTTPS domain. Google popup login does *not* work when opening `index.html` by double-clicking it as `file://`. You can use Netlify Drop (drag the extracted website folder to https://app.netlify.com/drop) or Firebase Hosting. Do not share the ZIP file as a direct webpage without deploying the HTML.
5. Go to https://console.firebase.google.com/project/studyflow-6b7b6/authentication/settings and add your deployed website's **exact hostname** under Authorized domains (e.g., `your-site.netlify.app`; do not add `https://` or paths). Firebase Hosting's default project domains are often already included, but check this list.
6. Visit the HTTPS URL and click **Continue with Google**. After sign-in, choose a blank timetable, the MITS CSE template, or import your older StudyFlow browser data.

## Optional: deploy via Firebase Hosting (requires Firebase CLI)

Use a terminal *inside this extracted folder*. If Firebase CLI is installed and you have signed in:

```sh
firebase login
firebase use studyflow-6b7b6
firebase deploy --only hosting
```

To also publish the provided Firestore rules with the CLI:

```sh
firebase deploy --only firestore:rules
```

For the easier manual path, just copy `firestore.rules` into the Firebase Console and use Netlify Drop for hosting. **No CLI or npm is required for that approach.**

## Test and troubleshoot

- **auth/operation-not-allowed:** Enable Google under Authentication → Sign-in method.
- **auth/unauthorized-domain:** Add your hosting hostname under Authentication → Settings → Authorized domains.
- **auth/popup-blocked:** Allow login popups from your site in the browser.
- **Cloud sync not working or permission denied:** Create Firestore and publish the provided rules; check that Authentication and Firestore are in the same project.
- **file:// page:** Upload the site to an HTTPS host; do not double-click the downloaded HTML for Google login.
- **Signed in but lost old data:** Use the Import previous StudyFlow data option on the **same browser where your older data is saved**. Export a JSON backup from the old site before migrating.

## Privacy & important limitations

- The `firebaseConfig` values in browser code are public client-side identifiers, **not** service-account secrets. Do not add administrative private keys to your HTML.
- Firestore security rules—not the API key—enforce account isolation. Do **not** use unrestricted Firestore rules.
- Student profiles within **one** signed-in Google account share that account's data. Independent students should sign in using **their own Google accounts** to keep data separate.
- Changes sync with last-write-wins behavior; avoid editing from two devices simultaneously. Export backups regularly.
- Requires internet for Google sign-in and cloud sync.
- The Google sign-in is implemented and Firebase config is included, but live login cannot be confirmed without the owner enabling Google, Firestore, and an authorized HTTPS domain.

Official Firebase docs: https://firebase.google.com/docs/auth/web/google-signin and https://firebase.google.com/docs/web/alt-setup and https://firebase.google.com/docs/firestore/security/get-started
