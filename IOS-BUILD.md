# iOS IPA Build — JMA&G BMS

This project is wrapped with Capacitor and is iOS IPA-ready.

- App name: **JMA&G BMS**
- App ID (Bundle ID): **com.jmag.bms**
- Web dir: `dist`
- iOS project: `ios/App/App.xcworkspace`
- App icon + splash: JM logo (`mobile-assets/ios/`)

## 1. Local prerequisites (macOS only)

- macOS with Xcode 15+ and Command Line Tools
- Node.js 20+, npm
- CocoaPods (`sudo gem install cocoapods`)
- An Apple Developer account + signing certificate / provisioning profile

## 2. Build locally

```bash
npm install
npm run build               # produces dist/
npx cap sync ios            # copies dist into ios/App/App/public
cd ios/App && pod install   # install CocoaPods
open App.xcworkspace        # open in Xcode
```

In Xcode:
1. Select the `App` target → Signing & Capabilities.
2. Pick your Team and confirm Bundle Identifier `com.jmag.bms`.
3. Product → Archive → Distribute App → App Store Connect (or Ad Hoc) → export `.ipa`.

## 3. Cloud build with Codemagic (no Mac required)

The included `codemagic.yaml` defines workflow **ios-capacitor** that:

1. Installs npm deps and builds the web app
2. Runs `npx cap sync ios` and `pod install`
3. Fetches App Store signing files via Codemagic integration
4. Archives + exports a signed `.ipa`
5. Uploads to TestFlight

### Steps to enable
1. Push this repo to GitHub / GitLab / Bitbucket.
2. Add the project in Codemagic ([codemagic.io](https://codemagic.io)).
3. In **Teams → Integrations**, connect **App Store Connect** (creates the `integration` reference used in `publishing`).
4. Create an environment variable group named `ios_signing` (can be left empty if using the integration; otherwise add `APP_STORE_CONNECT_ISSUER_ID`, `APP_STORE_CONNECT_KEY_IDENTIFIER`, `APP_STORE_CONNECT_PRIVATE_KEY`).
5. Make sure the Bundle ID `com.jmag.bms` exists in App Store Connect and an app record is created.
6. Trigger a build (push to `main` or run manually). The `.ipa` will be in the build artifacts.

## 4. Updating the icon / splash

Replace the JM logo files and re-run sync:

```bash
# icon (1024x1024 PNG, no transparency)
cp my-icon.png mobile-assets/ios/AppIcon.appiconset/Icon-App-1024x1024@1x.png
cp mobile-assets/ios/AppIcon.appiconset/Icon-App-1024x1024@1x.png \
   ios/App/App/Assets.xcassets/AppIcon.appiconset/AppIcon-512@2x.png

# splash (2732x2732 PNG)
magick my-splash.png -resize 2732x2732 -background white -gravity center -extent 2732x2732 \
   ios/App/App/Assets.xcassets/Splash.imageset/splash-2732x2732.png

npx cap sync ios
```

## 5. Troubleshooting

- **`pod install` fails on Apple Silicon**: `cd ios/App && arch -x86_64 pod install`
- **White screen on launch**: `npm run build && npx cap sync ios` to refresh `ios/App/App/public`.
- **Signing error in Xcode**: enable "Automatically manage signing" with your Team selected.
