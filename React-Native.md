# Commands
```sh
react-native run-android
react-native run-ios

react-native start

adb logcat
```
Clean
```sh
# Windows
cd android & gradlew clean & cd ..

# Linux
cd android ; sudo ./gradlew clean ; cd ..
```
Export (APK @ ./android/app/build/outputs/apk/app-release.apk):
```sh
# Windows
cd android & gradlew assembleRelease & cd ..

# Linux
cd android ; sudo ./gradlew assembleRelease ; cd ..
```

# ReactNative Install / Upgrade
```sh
npm install -g react-native-cli
npm install -g react-native-git-upgrade

react-native-git-upgrade
```

## 1. Project Start
```sh
react-native init app
react-native init app --version 0.55.4
```

## 2. Keytool & Signing
Generate key at C:\Program Files\Java\jdk1.8.0_141\bin (NO completar con datos en blanco). Place the myproject.keystore file under the android/app.
```sh
keytool -genkeypair -alias alias_name -keyalg RSA -validity 20000 -keystore project/android/app/myproject.keystore
```
Edit/Create ~/.gradle/gradle.properties (C:/Users/you/.gradle/):
```sh
MYAPP_RELEASE_STORE_FILE=myproject.keystore
MYAPP_RELEASE_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=*****
MYAPP_RELEASE_KEY_PASSWORD=*****
```
Edit /android/app/build.gradle
```sh
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            if (project.hasProperty('MYAPP_RELEASE_STORE_FILE')) {
                storeFile file(MYAPP_RELEASE_STORE_FILE)
                storePassword MYAPP_RELEASE_STORE_PASSWORD
                keyAlias MYAPP_RELEASE_KEY_ALIAS
                keyPassword MYAPP_RELEASE_KEY_PASSWORD
            }
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...
```

## AndroidManifest.xml (android/app/src/main)
Add xmlns:tools="http://schemas.android.com/tools" to manifest tag
```sh
<uses-permission android:name="android.permission.READ_PHONE_STATE" tools:node="remove" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" tools:node="remove" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" tools:node="remove" />
```

# Components
- React Navigation: <a href="https://reactnavigation.org">https://reactnavigation.org</a>
```sh
npm install --save react-navigation
```

- React Native Elements: <a href="https://github.com/react-native-training/react-native-elements">https://github.com/react-native-training/react-native-elements</a>
```sh
npm i --save react-native-vector-icons
react-native link react-native-vector-icons
npm i --save react-native-elements
```

- React Native FCM: <a href="https://github.com/evollu/react-native-fcm">https://github.com/evollu/react-native-fcm</a>
```sh
npm i --save react-native-fcm
react-native link react-native-fcm
```

- Firebase: <a href="https://www.npmjs.com/package/firebase">https://www.npmjs.com/package/firebase</a>
```sh
npm i --save firebase
```

- Firebase Functions: <a href="https://firebase.google.com/docs/functions/get-started">https://firebase.google.com/docs/functions/get-started</a>
```sh
npm i -g firebase-tools

firebase init
firebase deploy
```

- Facebook SDK: <a href="https://github.com/facebook/react-native-fbsdk">https://github.com/facebook/react-native-fbsdk</a>
```sh
# Follow Instructions
npm i --save react-native-fbsdk
react-native link react-native-fbsdk

# Facebook App Login - Android Key Hashes
keytool -exportcert -alias YOUR_RELEASE_KEY_ALIAS -keystore YOUR_RELEASE_KEY_PATH | openssl sha1 -binary | openssl base64
```

- Uninstall Component
```sh
react-native unlink component
npm uninstall --save component
```


# Software / Tools
It is impossible to use VirtualBox and Microsoft Hyper-V at the same time. Disable Hyper-V (search "Windows Features").
- <a href="https://www.virtualbox.org/">VirtualBox</a> (required)
- <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html">Java SE Development Kit (JDK) 8+</a> (required)
- <a href="https://developer.android.com/studio/install.html">Android Studio / Android SDK</a>
- <a href="http://romannurik.github.io/AndroidAssetStudio/">Android Icon Generator</a>
- <a href="https://makeappicon.com/">IOS Icon Generator</a>
- <a href="http://www.resizemypicture.com/">Image Resizer</a>
- <a href="https://www.youtube.com/watch?v=tnbOcpwJGa8">Tutorial Submit App IOS / XCODE</a>

# Errors
- npm ERR! tar.unpack untar error
```sh
npm cache clean
```
