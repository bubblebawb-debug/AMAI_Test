# AMAI for Android

This Android Studio project packages the AMAI HTML app in a native Android WebView shell. A GitHub Actions workflow is included so you can build a debug APK in the cloud without installing Android Studio.

## Easiest method: build the APK using GitHub in the cloud

1. Sign in to GitHub and open https://github.com/new.
2. In **Repository name**, enter `AMAI-Android`. Choose **Private** if you want the code visible only to you, then tap **Create repository**. You can leave other options at their defaults.
3. On the new repository page, choose **Add file → Upload files**.
4. Extract this ZIP on your device or computer. Upload the *contents* of the `AMAI_Android_Project` folder, not the ZIP itself. Include the hidden `.github/workflows/build-apk.yml` file. If the GitHub mobile site makes hidden folders difficult to upload, use a desktop browser or GitHub Desktop on a computer.
5. Commit the uploaded files to the `main` branch.
6. Open the repository's **Actions** tab. Select **Build AMAI APK**. If prompted, enable GitHub Actions.
7. Wait for the workflow to finish with a green check mark. Open the successful run and scroll to **Artifacts**.
8. Download **AMAI-debug-APK**. Extract that downloaded ZIP; inside is `app-debug.apk`.
9. Open the APK on your Android device and approve installing apps from that source if Android asks.

The workflow also runs automatically whenever you push a commit to `main` or `master`.

## Build locally in Android Studio

1. Install Android Studio and Android SDK Platform 35 / Build Tools.
2. Open this folder in Android Studio and let Gradle sync.
3. Choose **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
4. The debug APK will be at `app/build/outputs/apk/debug/app-debug.apk`.

## Important notes

- The GitHub workflow builds a **debug APK** for testing and personal installation. It is not a Play Store release build.
- The project targets Android 15 (API 35), with Android 8.0 (API 26) as the minimum.
- AMAI is an API-backed client. To get AI replies, configure a compatible model endpoint and API key in the app's model settings. Some providers restrict direct requests from WebViews.
- The HTML uses remote Google Fonts and Font Awesome CDN assets, so those resources need internet access. The app requests Internet permission.
- Chat/model data is stored in the WebView's local storage on-device. Exported backups are written to `Downloads/AMAI`.
- Keep API keys private. Do not put API keys directly into the source code or GitHub repository.

---

## Mobile UI overhaul (changelog)

**Touch / hover fixes**
- Profile edit pencil is always visible on touch; long-press a profile or chat for its menu; Edit sheet has Delete.
- Chat "⋯", model edit/delete and message actions are always visible on touch (tap a *user* bubble to reveal Copy/Edit).
- Every `:hover` style is wrapped in `@media (hover: hover)` so nothing sticks after a tap; `:active` press feedback added.
- Profile right-click menu was rendering *behind* the profile screen (z-index); fixed.

**Layout**
- Slide-in drawer (swipe right to open / left to close, tap scrim, back button), whole chat row is tappable and the drawer closes after picking a chat. New "Switch profile" button.
- Menus, model picker, modals and profile editor are bottom sheets; Settings is full-screen with swipeable tabs.
- Docked composer with 16px input, 44px buttons; Enter inserts a newline on touch keyboards (use the send button).
- Old mobile CSS targeted non-existent classes and made every `.settings-panel` fixed full-screen; replaced.

**Android shell**
- Fixed compile error in `onBackPressed()` (void used as boolean).
- Handles status bar / gesture bar / keyboard insets natively (Android 15 edge-to-edge), syncs bar colours with light/dark theme.
- Back button closes menus → sheets → drawer before leaving the app.
- Code-block download now uses the native saver (blob: downloads don't work in a WebView); clipboard fallback for Copy.
