# tauri


```shell
cargo install create-tauri-app --locked
cargo create-tauri-app

cd tauri-app
npm install
npm run tauri dev

<!--npm run tauri android dev-->
```

- 使用 Tauri CLI 手动创建
```shell
mkdir tauri-app
cd tauri-app
npm create vite@latest .

npm install -D @tauri-apps/cli@latest
<!--http://localhost:5173-->

npx tauri init

✔ What is your app name? tauri-app
✔ What should the window title be? tauri-app
✔ Where are your web assets located? ..
✔ What is the url of your dev server? http://localhost:5173
✔ What is your frontend dev command? pnpm run dev
✔ What is your frontend build command? pnpm run build


npx tauri dev
```

- android
```shell
paru -S android-studio truck rustup
<!--在 android-studio 安装 SDK 和 NDK-->
<!--paru -S android-ndk-->
<!--paru -S android-sdk-->

export ANDROID_HOME="$HOME/Android/Sdk"
export NDK_HOME="$ANDROID_HOME/ndk/$(ls -1 $ANDROID_HOME/ndk)"

<!--Android SDK Platform-->
<!--Android SDK Platform-Tools-->
<!--NDK (Side by side)-->
<!--Android SDK Build-Tools-->
<!--Android SDK Command-line Tools-->

rustup default stable
rustup target add aarch64-linux-android armv7-linux-androideabi i686-linux-android x86_64-linux-android
rustup target add wasm32-unknown-unknown


cargo tauri android init
cargo tauri android dev
cargo tauri android build
cargo tauri android build --target aarch64

# 签名
keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  black
What is the name of your organizational unit?
  [Unknown]:  black
What is the name of your organization?
  [Unknown]:  black
What is the name of your City or Locality?
  [Unknown]:  black
What is the name of your State or Province?
  [Unknown]:  black
What is the two-letter country code for this unit?
  [Unknown]:  black
Is CN=black, OU=black, O=black, L=black, ST=black, C=black correct?
  [no]:  y

Generating 2,048 bit RSA key pair and self-signed certificate (SHA256withRSA) with a validity of 10,000 days
        for: CN=black, OU=black, O=black, L=black, ST=black, C=black
Enter key password for <upload>
        (RETURN if same as keystore password):
[Storing /home/black/upload-keystore.jks]

Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore /home/black/upload-keystore.jks -destkeystore /home/black/upload-keystore.jks -deststoretype pkcs12".


nvim src-tauri/gen/android/keystore.properties
password=xxxxxxxxxx
keyAlias=upload
storeFile=/home/black/upload-keystore.jks

nvim src-tauri/gen/android/app/build.gradle.kts

import java.io.FileInputStream

signingConfigs {
    create("release") {
        val keystorePropertiesFile = rootProject.file("keystore.properties")
        val keystoreProperties = Properties()
        if (keystorePropertiesFile.exists()) {
            keystoreProperties.load(FileInputStream(keystorePropertiesFile))
        }

        keyAlias = keystoreProperties["keyAlias"] as String
        keyPassword = keystoreProperties["password"] as String
        storeFile = file(keystoreProperties["storeFile"] as String)
        storePassword = keystoreProperties["password"] as String
    }
}

buildTypes {
    getByName("release") {
        signingConfig = signingConfigs.getByName("release")
    }
}

```
