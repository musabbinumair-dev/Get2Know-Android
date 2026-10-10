# Native Android APK Build: Get-to-Know-You

Convert Get-to-Know-You into a proper native Android APK project featuring a complete Android Studio Gradle configuration, AndroidManifest, adaptive launcher icons, and APK build pipeline, while strictly maintaining 100% of the game's existing visual design, pastel illustrations, and feature set.

### User Review & Critical Decisions

> [!IMPORTANT]
> **Environment & APK Build Architecture**:
> This AI Studio workspace runs in a Node.js web container where the live preview is hosted on port 3000. To deliver a genuine native Android APK without breaking the live preview:
> 1. We will generate the complete native **Android Studio Project** inside `android/` (with `build.gradle`, `app/build.gradle`, `AndroidManifest.xml`, `MainActivity`, app icons, splash screens, and Gradle build scripts).
> 2. The Android project wraps the game using Android's native WebView container (Capacitor Android runtime), producing an installable `.apk` package (`app-debug.apk`).
> 3. We will provide an automated APK build script and GitHub Actions CI configuration to automatically compile and release the `app-debug.apk` directly from your repository.

- **Confirmed Decision 1 (Android APK Target)**: Generate the complete native Android project in `android/` capable of building a standalone `app-debug.apk`.
- **Confirmed Decision 2 (Zero Design Change)**: Keep 100% of the original pastel styling, Nunito typography, avatar blobs, animations, and game layouts exactly the same.
- **Confirmed Decision 3 (Feature Completeness)**: Full feature parity (Daily Question, Guessing, Sync Reveal, Memory Wall with emoji reactions, Lobby, Profile, and Settings).
- **Confirmed Decision 4 (Android Native Features)**: Android back-button handling, status bar color matching (`#FAF6EA`), native splash screen, and adaptive launcher icons.

---

### 1. Overview & Core Concept

- **What It Delivers**:
  - A production-ready native Android project located in `android/` with Gradle configuration, ready to build with `./gradlew assembleDebug` into `app-debug.apk`.
  - Android-specific metadata: `package_name: "com.get2know.app"`, versioning, SDK target (compileSdk 34+, minSdk 22), permissions, and hardware acceleration.
  - Native Android app icons (`res/mipmap-*`), splash screen configurations, and status bar integration.
  - GitHub Actions APK builder workflow (`.github/workflows/build-apk.yml`) that compiles the APK automatically on push and provides an instant downloadable `.apk` artifact.
- **Target Audience**: Android users who want a native APK installed directly on their device via APK sideloading or Android Studio.

---

### 2. User Experience & Visual Design

- **App Launch & Splash Screen**:
  - Launches with a native Android splash screen styled in cream (`#FAF6EA`) with the centered Get-to-Know-You duo starburst branding.
  - Smooth transition directly into the application shell with zero visible web browser UI.
- **Android System UI Integration**:
  - Native status bar colored `#FAF6EA` with dark icons to match the cream theme.
  - Native navigation bar colored `#FAF6EA`.
  - Native Android hardware/gesture back-button support hooked into the router navigation history.
- **Visual Design**:
  - Every single screen, button, color token, font, and illustration remains 100% identical to the current design.

---

### 3. Key Product Decisions & Trade-Offs

- **Decision 1: Full Native Android Project (`android/`) via Capacitor Android Runtime**:
  - *Chosen Approach*: Configure `@capacitor/core`, `@capacitor/android`, and export the complete `android/` project directory.
  - *Why*: Enables compiling a genuine `.apk` through standard Gradle (`assembleDebug`) while keeping the existing React codebase and assets completely functional.
  - *Alternatives Considered*: Pure Kotlin rewrite from scratch (would lose live preview in AI Studio Web container and risk breaking nuanced animation/asset logic).
- **Decision 2: Automated APK Build Pipeline (GitHub Actions)**:
  - *Chosen Approach*: Include `.github/workflows/build-apk.yml` that runs Gradle `assembleDebug` with Temurin JDK 17/21 and uploads `app-debug.apk` as a downloadable release/artifact.
  - *Why*: Allows anyone to download the compiled `.apk` directly without needing Android Studio installed locally.
- **Decision 3: Dual-Mode Compatibility**:
  - *Chosen Approach*: The project maintains its Vite development and compilation scripts (`npm run dev`, `npm run build`), ensuring AI Studio's preview remains active at all times.

---

### 4. Technical Architecture & Android Project Structure

```
┌────────────────────────────────────────────────────────┐
│                   Android APK Structure                │
│                                                        │
│  com.get2know.app                                      │
│  ├── AndroidManifest.xml                               │
│  ├── java/com/get2know/app/MainActivity.java (or .kt)  │
│  ├── res/                                              │
│  │   ├── mipmap-*/ (Adaptive Launcher Icons)           │
│  │   ├── values/styles.xml & colors.xml (#FAF6EA)      │
│  │   └── drawable/splash.xml                           │
│  ├── build.gradle & app/build.gradle (AGP + Gradle)    │
│  └── assets/public/ (Pre-bundled React Game Assets)    │
└──────────────────────────┬─────────────────────────────┘
                           │ builds via Gradle
┌──────────────────────────▼─────────────────────────────┐
│             Output: app-debug.apk                      │
│      (Installable on any Android 8.0+ device)          │
└────────────────────────────────────────────────────────┘
```

- **File Plan**:
  - `capacitor.config.json` (App ID: `com.get2know.app`, App Name: `Get-to-Know-You`)
  - `android/` folder with complete Gradle project and manifest
  - `.github/workflows/build-apk.yml` for automated APK compilation and download
  - Android back-button listener integrated in `App.tsx`
