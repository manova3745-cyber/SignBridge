# SignBridge — Native App Build

This folder wraps the SignBridge web app (`www/index.html`) into a real
Android/iOS app using [Capacitor](https://capacitorjs.com). As a native
app, the camera runs through the OS's own permission system — not a
browser sandbox — so live camera detection works properly here.

## Prerequisites (on your own computer)
- [Node.js](https://nodejs.org) (v18+)
- For Android: [Android Studio](https://developer.android.com/studio)
- For iOS: a Mac with Xcode

## Steps — Android (APK)

```bash
cd signbridge-app
npm install
npx cap add android
npx cap sync android
npx cap open android
```

This opens the project in Android Studio. Then:
1. Open `android/app/src/main/AndroidManifest.xml` and make sure this line
   is present (add it just above `</manifest>` if missing):
   ```xml
   <uses-permission android:name="android.permission.CAMERA" />
   ```
2. In Android Studio: **Build → Build Bundle(s)/APK(s) → Build APK(s)**.
3. The APK appears under `android/app/build/outputs/apk/debug/`.
   Install it on your phone, or connect your phone via USB and hit ▶ Run.

To publish on the Play Store later, generate a signed release build
(**Build → Generate Signed Bundle / APK**) instead of a debug APK.

## Steps — iOS (Mac only)

```bash
cd signbridge-app
npm install
npx cap add ios
npx cap sync ios
npx cap open ios
```

In Xcode: open `ios/App/App/Info.plist` and add a
`NSCameraUsageDescription` key with a short reason string (e.g. "Used
to detect hand signs"), then Run on a connected device (the simulator
has no camera).

## After editing the web app

If you change `www/index.html` later, re-sync before rebuilding:
```bash
npx cap sync
```

## Notes
- All detection/voice logic runs entirely on-device (no server, no
  network calls) — same as the web version.
- The on-device gesture estimator recognizes a starter set (fist, one
  finger, two fingers, open palm). For full 26-letter accuracy, swap
  this for a trained model (e.g. MediaPipe Hands or a TFLite model)
  bundled into the native app.
