# Wireless Charge Controller — no-root prototype

This is a basic Android Studio Kotlin project for monitoring (not disabling) wireless charging on a phone.

## What it does
- Reads Android's sticky battery broadcast and identifies `BATTERY_PLUGGED_WIRELESS`.
- Reads the magnetic-field sensor and lets you save a baseline while the phone is away from the stand.
- Uses NFC reader mode while the app is open. Attach a separate NFC tag to the stand and tap the phone near it.
- Shows an on-screen warning when wireless charging is detected.

## Important limitation
There is no public Android API that lets an ordinary third-party app disable the wireless charging transmitter/receiver hardware on demand. This prototype does **not** block or stop charging. To stop charging, disconnect power to the stand or use a non-powered magnetic holder.

## Build
1. Install Android Studio (current stable) and Android SDK Platform 35.
2. Open this folder in Android Studio.
3. Allow Gradle sync to finish.
4. Connect the Moto Edge 60 Pro with USB debugging enabled.
5. Press Run.

The project uses Android Gradle Plugin 8.7.3 and Kotlin 2.0.21. Android Studio may suggest a compatible Gradle version if your installation needs one.

## Test
1. Open the app and allow it to run in the foreground.
2. With the phone away from the magnetic stand, tap **Calibrate**.
3. Move the phone close to the stand and watch the magnetic-field change.
4. Enable NFC in Android settings, if available. Place an NFC tag on the stand and tap the phone near it.
5. Connect a wireless charger and confirm that the app reports wireless charging.

## Notes
- Magnetic-field thresholds vary by phone, case, stand, and environment. The 20 µT threshold is only a starting point, not a certified stand detector.
- NFC only works if an NFC tag is physically attached to the stand; it does not detect a passive magnet by itself.
- Battery status monitoring is most reliable while this app is running in the foreground in this prototype.


## Cloud APK build with Codemagic (no Android Studio on your laptop)

This project includes `codemagic.yaml` for a cloud debug-APK build.

1. Create/sign in to a GitHub account.
2. Create a **private** GitHub repository (recommended).
3. Upload the contents of this project folder to the repository (not the ZIP file itself).
4. Sign in to Codemagic at https://codemagic.io/ and connect that GitHub repository.
5. Add the app as a Native Android project and start the `android-debug` workflow.
6. When the build succeeds, download the APK from the build's Artifacts section.

Codemagic needs permission to read the repository. Do not upload passwords, signing keys, or other secrets to the repository. This workflow creates a debug APK for personal testing; a production release should use a private signing key stored securely in Codemagic.

## Free APK build using GitHub Actions

The repository includes `.github/workflows/android-apk.yml`.

1. Create a **public** repository on GitHub.
2. Extract this ZIP on your computer.
3. In the new repository, choose **Add file → Upload files** and upload all extracted project files and folders, including the hidden `.github` folder. Commit to `main`.
4. Open the repository's **Actions** tab. Select **Build Wireless Charge Controller APK** and run it if the workflow has not started automatically.
5. Open the completed workflow run, find **Artifacts**, and download `WirelessChargeController-debug-apk`.
6. Extract the downloaded artifact; it contains `app-debug.apk`.

GitHub-hosted standard runners are free for public repositories. Keep in mind the repository source will be public. Do not add signing keys, passwords, or personal secrets.

This debug APK is for testing. The app monitors wireless charging and sensor readings; it does not disable the wireless charging hardware.
