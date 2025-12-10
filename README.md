ঠিক আছে! আমি আপনার জন্য একটি **professional-ready README.md** বানিয়ে দিচ্ছি যা পুরো Flutter WebView app এর জন্য **setup, build, release, splash & icon, keystore** সব cover করবে।

---

# `README.md` for Bojoit Flutter WebView App

````markdown
# Bojoit Flutter WebView App

A professional Flutter WebView app that loads `https://bojoit.com`.  
This project includes splash screen, launcher icon, connectivity handling, and Play Store ready build setup.

---

## Table of Contents

- [Project Setup](#project-setup)
- [Dependencies](#dependencies)
- [Flutter Configuration](#flutter-configuration)
- [Splash Screen & App Icon](#splash-screen--app-icon)
- [Keystore Setup for Release](#keystore-setup-for-release)
- [Build Commands](#build-commands)
- [Install APK / Play Store](#install-apk--play-store)
- [Troubleshooting](#troubleshooting)

---

## Project Setup

1. Clone or download the project:

```bash
git clone <your-repo-url>
cd bojoit
````

2. Install Flutter dependencies:

```bash
flutter pub get
```

3. Ensure Android SDK, JDK, and device/emulator are ready:

```bash
flutter doctor
```

---

## Dependencies

* Flutter SDK ≥ 3.9.0
* [flutter_inappwebview](https://pub.dev/packages/flutter_inappwebview)
* [webview_flutter](https://pub.dev/packages/webview_flutter)
* [connectivity_plus](https://pub.dev/packages/connectivity_plus)
* [url_launcher](https://pub.dev/packages/url_launcher)
* [flutter_launcher_icons](https://pub.dev/packages/flutter_launcher_icons)
* [flutter_native_splash](https://pub.dev/packages/flutter_native_splash)

All dependencies are already listed in `pubspec.yaml`.

---

## Flutter Configuration

**main.dart**:

* Loads your website in a full-screen WebView.
* Handles **connectivity changes**.
* Handles **back button navigation**.
* Shows **progress indicator** while loading.

Example snippet:

```dart
InAppWebView(
  initialUrlRequest: URLRequest(url: Uri.parse("https://bojoit.com")),
  initialOptions: InAppWebViewGroupOptions(
    crossPlatform: InAppWebViewOptions(
      javaScriptEnabled: true,
      mediaPlaybackRequiresUserGesture: false,
    ),
  ),
  onWebViewCreated: (controller) => webViewController = controller,
)
```

---

## Splash Screen & App Icon

### App Icon

* Path: `assets/icon/app_icon.png`
* Configured in `pubspec.yaml` with **flutter_launcher_icons**:

```yaml
flutter_icons:
  android: true
  ios: true
  image_path: "assets/icon/app_icon.png"
```

* Generate icons:

```bash
flutter pub run flutter_launcher_icons:main
```

### Splash Screen

* Path: `assets/splash/logo.png`
* Configured in `pubspec.yaml` with **flutter_native_splash**:

```yaml
flutter_native_splash:
  color: "#ffffff"
  image: assets/splash/logo.png
  android: true
  ios: true
  web: false
```

* Generate splash:

```bash
flutter pub run flutter_native_splash:create
```

---

## Keystore Setup for Release

1. Generate keystore:

```bash
keytool -genkey -v -keystore android/app/bojoit.jks -keyalg RSA -keysize 2048 -validity 10000 -alias bojoit
```

2. Create `android/key.properties`:

```properties
storePassword=<your_keystore_password>
keyPassword=<your_key_password>
keyAlias=bojoit
storeFile=bojoit.jks
```

3. Configure `android/app/build.gradle.kts` to use release signing:

```kotlin
signingConfigs {
    create("release") {
        val keystorePropertiesFile = rootProject.file("key.properties")
        val keystoreProperties = java.util.Properties().apply {
            load(java.io.FileInputStream(keystorePropertiesFile))
        }

        storeFile = file(keystoreProperties["storeFile"] as String)
        storePassword = keystoreProperties["storePassword"] as String
        keyAlias = keystoreProperties["keyAlias"] as String
        keyPassword = keystoreProperties["keyPassword"] as String
    }
}

buildTypes {
    release {
        signingConfig = signingConfigs.getByName("release")
    }
}
```

---

## Build Commands

Run from **project root**:

### Clean + Get Dependencies

```bash
flutter clean
flutter pub get
```

### Build APK

```bash
flutter build apk --release
```

* Output: `build/app/outputs/flutter-apk/app-release.apk`

### Build AAB (Play Store)

```bash
flutter build appbundle --release
```

* Output: `build/app/outputs/bundle/release/app-release.aab`

### Optional: Split APK (32-bit & 64-bit)

```bash
flutter build apk --release --target-platform android-arm,android-arm64 --split-per-abi
```

* Output:

```
app-armeabi-v7a-release.apk
app-arm64-v8a-release.apk
```

---

## Install APK / Play Store

* Enable **Unknown Sources** on your Android device.
* Install APK directly: `app-release.apk`
* Upload AAB to Play Store for production release.

---

## Troubleshooting

1. **“There was a problem parsing the package”**

   * Use **split APK** for 32-bit devices.
   * Ensure **minSdkVersion ≥ 21**.
   * Uninstall old debug version before installing release APK.

2. **keytool not recognized**

   * Install JDK and set `JAVA_HOME` environment variable.
   * Add `%JAVA_HOME%\bin` to system PATH.

3. **Flutter build errors**

   * Run:

```bash
flutter clean
flutter pub get
flutter doctor
```

---

## Notes

* Keep **bojoit.jks** safe. Needed for all future updates.
* Passwords in `key.properties` should **never be shared**.
* Splash & Icon can be updated in `assets/` and regenerated using the above commands.

---

**Developed by Ashik Ahmed**

```

---

আমি চাইলে আমি এই README এর সাথে **project-ready folder structure + copy-paste build commands** সহ একটি **one-line ready guide** বানিয়ে দিতে পারি, যা নতুন developer একবারে setup করতে পারবে।  

আপনি কি সেটা চান?
```
