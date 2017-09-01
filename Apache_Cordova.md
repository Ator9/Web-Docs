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


## 2. config.xml
Change id
```xml
<widget id="io.cordova.your_app_name">
    <name>App Name</name>
</widget>
```
Security
```sh
<allow-navigation href="*" />
```

## 3. Start
```sh
cordova run android
```

## 4. Java Keytool & Final Build
Generate key at C:\Program Files\Java\jdk1.8.0_141\bin (NO completar con datos en blanco)
```sh
keytool -genkeypair -alias alias_name -keyalg RSA -validity 20000 -keystore H:\project\key.keystore
```
/platforms/android/build/outputs/apk/android-release-unsigned.apk
```sh
cordova build android --release
cordova build android --release -- --keystore=../key.keystore --storePassword=password --alias=alias_name --password=password
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

