Apache Cordova

# Commands
## 1. Creation
```sh
cordova create app

cordova platform ls
cordova platform add android
cordova platform add ios

cordova requirements
```
<a href="https://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html#requirements-and-support">Android Requirements</a> - <a href="https://cordova.apache.org/docs/en/latest/guide/platforms/ios/index.html#requirements-and-support">iOS Requirements</a>


## 2. Start
```sh
cordova run android
```

## 3. config.xml
```sh
<allow-navigation href="*" />
```

## 4. Final Build
/platforms/android/build/outputs/apk/android-release-unsigned.apk
```sh
cordova build android --release
```

# AVD
```sh
android.bat create avd -n "test" -k "system-images;android-25;google_apis;x86"
```

# Tools & Fixes
- virtmgmt.msc

- <a href="https://www.visualstudio.com/vs/msft-android-emulator/" target="_blank">Visual Studio Emulator for Android</a> 
- Cannot launch emulator (Linux)

export ANDROID_EMULATOR_USE_SYSTEM_LIBS=1
<a href="https://stackoverflow.com/questions/35911302/cannot-launch-emulator-on-linux-ubuntu-15-10" target="_blank">Thread</a> 

- Enable virtualization in BIOS

