ঠিক আছে! আমি আপনাকে একটি **complete professional-ready `README.md`** বানিয়ে দিচ্ছি যা **Flutter default app** এর জন্য হবে, এবং এতে থাকবে **keystore setup, release build, splash & icon setup**।

---

# `README.md` for Flutter Default App (Release Ready with Keystore)

````markdown
# Flutter Default App - Release Ready Setup

This is a Flutter default app template configured for **release build** with **keystore**, **splash screen**, **launcher icon**, and **Play Store-ready setup**.

---

## Table of Contents

- [Project Setup](#project-setup)
- [Dependencies](#dependencies)
- [Keystore Setup](#keystore-setup)
- [Build Commands](#build-commands)
- [Splash Screen & App Icon](#splash-screen--app-icon)
- [Install APK / Play Store](#install-apk--play-store)
- [Troubleshooting](#troubleshooting)

---

## Project Setup

1. Clone or download the project:

```bash
git clone <your-repo-url>
cd flutter_default_app
```
````

2. Install Flutter dependencies:

```bash
flutter pub get
```

3. Check Flutter environment:

```bash
flutter doctor
```

---

## Dependencies

The default Flutter app requires only:

```yaml
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.8
```

- Additional dependencies can be added as needed.

---

## Keystore Setup

Flutter release build requires a **signing key**.

1. **Generate Keystore**

Open terminal at project root and run:

```bash
keytool -genkey -v -keystore android/app/keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias appname
```

- Enter keystore password (example: `flutterpass`)
- Enter key password (example: `flutterpass`)
- Fill Distinguished Name or leave default values.

> Output file: `android/app/keystore.jks`

2. **Create `key.properties`** in `android/` folder:

```properties
storePassword=flutterpass
keyPassword=flutterpass
keyAlias=flutterapp
storeFile=keystore.jks
```

---

## Configure `android/app/build.gradle.kts`

Add release signing configuration:

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
        isMinifyEnabled = false
        isShrinkResources = false
        signingConfig = signingConfigs.getByName("release")
    }
}
```

---

## Splash Screen & App Icon

1. Place your splash image:

```
assets/splash/logo.png
```

2. Place your app icon:

```
assets/icon/app_icon.png
```

3. Configure `pubspec.yaml`:

```yaml
flutter:
  uses-material-design: true
  assets:
    - assets/splash/
    - assets/icon/

flutter_launcher_icons:
  android: true
  ios: true
  image_path: "assets/icon/app_icon.png"

flutter_native_splash:
  color: "#ffffff"
  image: assets/splash/logo.png
  android: true
  ios: true
```

4. Generate icons & splash:

```bash
flutter pub run flutter_launcher_icons:main
flutter pub run flutter_native_splash:create
```

---

## Build Commands

Run from project root:

```bash
flutter clean
flutter pub get

# Build release APK
flutter build apk --release

# Build Play Store ready AAB
flutter build appbundle --release

# Optional: Split APK for 32-bit & 64-bit
flutter build apk --release --target-platform android-arm,android-arm64 --split-per-abi
```

- APK Output: `build/app/outputs/flutter-apk/app-release.apk`
- AAB Output: `build/app/outputs/bundle/release/app-release.aab`

---

## Install APK / Play Store

- Enable **Unknown Sources** on your Android device.
- Install APK directly (`app-release.apk`)
- Upload **AAB** to Play Store for production release.

---

## Troubleshooting

1. **keytool not recognized**

- Install JDK and set `JAVA_HOME` environment variable.
- Add `%JAVA_HOME%\bin` to PATH.

2. **Parsing package error**

- Use split APK for 32-bit devices.
- Ensure `minSdkVersion >= 21`.

3. **Flutter build errors**

```bash
flutter clean
flutter pub get
flutter doctor
```

---

## Notes

- Keep `keystore.jks` safe for all future updates.
- Do not share `key.properties` publicly.
- Splash and icons can be updated in `assets/` and regenerated.

---

**Developed by Your Name**

```

```
