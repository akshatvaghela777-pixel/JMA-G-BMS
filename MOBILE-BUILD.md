# JMA&G BMS Mobile Build

The prepared logo and splash assets are in `mobile-assets/android` and
`mobile-assets/ios/AppIcon.appiconset`.

## Android APK

```powershell
npm install @capacitor/core @capacitor/cli @capacitor/android @capacitor/splash-screen
npx cap add android
npm run build
npx cap sync android
Copy-Item mobile-assets\android\* android\app\src\main\res -Recurse -Force
cd android
.\gradlew assembleDebug
```

APK output:
`android/app/build/outputs/apk/debug/app-debug.apk`

Requires Java 17+, Android SDK, and Android Studio/Gradle.

## iPhone and iPad

Run on macOS with Xcode:

```bash
npm install @capacitor/core @capacitor/cli @capacitor/ios @capacitor/splash-screen
npx cap add ios
npm run build
npx cap sync ios
cp -R mobile-assets/ios/AppIcon.appiconset ios/App/App/Assets.xcassets/
npx cap open ios
```

Select an Apple Developer Team and provisioning profile in Xcode before
creating an IPA archive.
