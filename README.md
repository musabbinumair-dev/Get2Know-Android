# Get-to-Know-You

A private two-person daily question game for two best friends. Two friends. One question a day.

This repository includes both the live web application and the complete **native Android project** ready to build as a native Android APK.

---

## 📱 Building the Native Android APK

### Option A: Automatic Build via GitHub Actions (Zero Setup)
1. Push this repository to GitHub.
2. Go to the **Actions** tab on GitHub.
3. Select the **Build Android APK** workflow and click **Run workflow**.
4. Once completed, download the generated **Get-to-Know-You-Debug-APK** artifact containing `app-debug.apk` directly to your phone or computer.

### Option B: Build with Android Studio
1. Install dependencies and sync web assets:
   ```bash
   npm install
   npm run build:android
   ```
2. Open the `android/` directory in **Android Studio**.
3. Click **Build > Build Bundle(s) / APK(s) > Build APK(s)** or click the **Run** button to launch on your connected Android device / emulator.
4. The generated APK will be at:
   `android/app/build/outputs/apk/debug/app-debug.apk`

### Option C: Build from Command Line (Terminal)
```bash
npm run build:android
cd android
./gradlew assembleDebug
```
The APK is generated at:
`android/app/build/outputs/apk/debug/app-debug.apk`

---

## 💻 Running the Web App Locally

**Prerequisites:** Node.js 18+

1. Install dependencies:
   ```bash
   npm install
   ```
2. Run development server:
   ```bash
   npm run dev
   ```
3. Open http://localhost:3000
