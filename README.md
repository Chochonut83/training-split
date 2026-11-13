# Training Split

A phone-friendly app for tracking who made it to each training session and splitting the cost.
Everyone who enters the same group code sees the same numbers, updated live.

It runs for free on **GitHub Pages**, and the shared data lives in **Firebase** (Firestore) on the free Spark plan. No credit card is needed.

---

## What's in this folder

| File | What it is |
|---|---|
| `index.html` | The app |
| `firebase-config.js` | Your Firebase connection settings. **You'll edit this one file.** |
| `firestore.rules` | Security rules to paste into Firebase. These don't need to be uploaded to GitHub. |
| `manifest.webmanifest`, `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`, `apple-touch-icon.png` | The home screen name and the Evoke Training Split icon |

---

## Part 1: Set up Firebase (about 10 minutes)

### 1. Create the project
1. Go to **https://console.firebase.google.com** and sign in with a Google account.
2. Click **Create a project** (or **Add project**).
3. Name it `training-split` and click **Continue**.
4. When asked about Google Analytics, switch it **off**, then click **Create project**.
5. When it says your project is ready, click **Continue**.

### 2. Create the database
1. In the left menu, open **Build** (or **Databases & Storage**) and click **Firestore Database**.
2. Click **Create database**.
3. Choose a location. **northamerica-northeast1 (Montréal)** is closest to Toronto. You can't change it later.
4. Choose **Start in production mode** and click **Create**.

### 3. Paste in the security rules
1. Still in Firestore Database, click the **Rules** tab.
2. Delete everything in the editor.
3. Open `firestore.rules` from this folder, copy all of it, and paste it in.
4. Click **Publish**.

These rules let someone open a group only if they already know its code. Nobody can list or browse the groups.

### 4. Turn on anonymous sign-in
The app signs each phone in quietly in the background. No one creates an account.
1. In the left menu, open **Build** (or **Security**) and click **Authentication**.
2. Click **Get started**.
3. On the **Sign-in method** tab, click **Anonymous**, switch it **on**, and click **Save**.

### 5. Get your connection settings
1. Click the **gear icon** next to *Project Overview* (top left), then **Project settings**.
2. Scroll to **Your apps** and click the **web icon** `</>`.
3. Name the app `training-split`. **Don't** tick "Also set up Firebase Hosting". Click **Register app**.
4. You'll see a block of code containing `const firebaseConfig = { ... }`. Keep this page open. You'll copy six values from it in Part 2, step 3.

---

## Part 2: Put the app on GitHub (about 10 minutes)

### 1. Create the repository
1. Go to **https://github.com/new**.
2. Repository name: `training-split`
3. Choose **Public**. GitHub Pages is free only for public repositories, and your data stays private in Firebase, not on GitHub.
4. Click **Create repository**.

### 2. Upload the files
1. On the new, empty repository page, click the **uploading an existing file** link.
2. Drag in every file from this folder: `index.html`, `firebase-config.js`, `manifest.webmanifest`, the four `.png` icons and `README.md`. (`firestore.rules` is optional here; it only needs to be pasted into Firebase.)
3. Click **Commit changes**.

### 3. Connect it to Firebase
1. In the repository, click `firebase-config.js`, then the **pencil icon** (Edit).
2. Replace each `PASTE_...` value with the matching value from the Firebase page you kept open (Part 1, step 5). Keep the quotes. It should end up looking like this, with your own values:
   ```js
   export const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "training-split-1a2b3.firebaseapp.com",
     projectId: "training-split-1a2b3",
     storageBucket: "training-split-1a2b3.firebasestorage.app",
     messagingSenderId: "123456789012",
     appId: "1:123456789012:web:abc123..."
   };
   ```
3. Click **Commit changes** (twice).

These values are meant to be public. The security rules are what protect your data.

### 4. Turn on GitHub Pages
1. In the repository, click **Settings**, then **Pages** in the left menu.
2. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
3. Set **Branch** to `main` and the folder to `/ (root)`, then click **Save**.
4. Wait 1 to 2 minutes and refresh. A box at the top will show your site address, which looks like:
   **https://YOUR-GITHUB-NAME.github.io/training-split/**

### 5. Allow your site in Firebase
1. Back in Firebase, go to **Authentication**, then the **Settings** tab, then **Authorized domains**.
2. Click **Add domain** and enter `YOUR-GITHUB-NAME.github.io` (no `https://`, no `/training-split`).
3. Click **Add**.

---

## Part 3: Start using it

1. Open your site address on your phone.
2. Tap **Start a new group**. You'll get a 12-character group code, like `K7QM-3XPT-2WAD`.
3. Tap **Copy code** and text it to everyone in the group.
4. Each person opens the same site address, enters the code and taps **Join**.
5. **Add it to the home screen:**
   - **iPhone or iPad:** in Safari, tap the Share button, then **Add to Home Screen**.
   - **Android:** in Chrome, tap ⋮, then **Add to Home screen** or **Install app**.

   The first time someone opens the home screen icon, they enter the code once more.

**Bringing over your old data:** In the old Claude tracker, go to Settings and tap **Copy backup**. In the new app, go to Settings, then **Bring in data from the old tracker**, paste it and tap **Replace this group's data**.

---

## Good to know

- **Anyone with the code can view and change everything.** Share it only with the group. If it leaks, start a new group, import a backup and share the new code.
- **Offline:** changes made with no signal are saved on the phone and sync when it reconnects.
- **Two people tapping at once** is fine. Each tap only changes its own checkbox or payment, so nothing gets overwritten.
- **Making changes to the app later:** edit `index.html` on GitHub. The live site updates within a minute or two.
- **Cost:** the Firebase free plan allows 50,000 reads and 20,000 writes a day, far more than a training group will ever use.
