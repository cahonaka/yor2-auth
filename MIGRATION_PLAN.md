# YOR 2 — Capacitor migration plan

## Goal
Keep the existing YOR 2 HTML/CSS/JavaScript application and Firebase project, and package the same app for Android and iOS with Capacitor. The current production branch and GitHub Pages site must remain untouched while the migration is prepared.

## Current baseline
- Repository: cahonaka/yor2-auth
- Existing web entry point: index.html
- Existing backend: Firebase Authentication and Cloud Firestore
- Current UI and app logic are in a single HTML file.
- Current published web app remains on the default branch.

## Migration strategy
1. **Protect the working app**
   - Work on the separate `capacitor-migration` branch.
   - Keep the default branch and current GitHub Pages deployment unchanged.
   - Preserve the existing Firebase project, users, authentication, profiles, chats, and message data.

2. **Prepare a Capacitor-friendly project structure**
   - Introduce a package manifest and Capacitor configuration.
   - Put the existing web app into a dedicated web-assets directory (initially `www/index.html`).
   - Keep changes to the existing app minimal; do not rewrite the UI framework.

3. **Add native targets**
   - Add Android and iOS Capacitor platforms.
   - Configure the same app identity, display name, icons, splash screen, status bar, keyboard behavior, and safe areas.
   - Add native plugins only when required (camera/gallery, push notifications, file system, share, haptics).

4. **Stabilize core messenger behavior**
   - Verify login/registration and existing Firebase data access.
   - Fix and test sending messages, keyboard focus, and scroll-to-latest.
   - Verify avatar selection and persistence; avoid storing oversized base64 images in Firestore.

5. **Add features in safe stages**
   - Stage A: reliable text chat, profiles, search, chat list.
   - Stage B: image/file sharing and push notifications.
   - Stage C: voice messages and audio/video calls (requires additional media/signaling infrastructure and testing).

6. **Build and release**
   - Android: build and test an APK/AAB using Android Studio.
   - iOS: build and test with Xcode on macOS; App Store distribution requires Apple Developer enrollment.
   - A Mac with Xcode is required for a local iOS build; a suitable cloud build service is an alternative.

## Important constraints
- Capacitor 8 currently requires Node.js 22+, Android Studio 2025.2.1+ for Android, and macOS with Xcode 26+ for local iOS builds. Confirm tool versions before installing.
- The user's existing MIT App Inventor APK is not the source project for this migration; the web app in this repository is the source of truth.
- Do not change Firebase API keys, authentication providers, Firestore rules, or production deployment as part of initial scaffolding.
- Do not claim an APK or iOS app has been built until a real build and device test succeeds.

## First implementation milestone
Create a Capacitor shell on this branch, copy the existing web entry point into `www/index.html`, and validate that the web version still behaves as before before attempting native builds.
