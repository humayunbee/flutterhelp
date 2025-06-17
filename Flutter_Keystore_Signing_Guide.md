
# 🔐 Flutter Android App Signing Guide with Keystore

This guide will help you generate a keystore and configure your Flutter project to build a signed Android App Bundle for Play Store deployment.

---

## 📌 Step 1: Generate Keystore File

Create a Folder credentail:

Run the following command in terminal (adjust the file path as needed):

```bash
keytool -genkey -v -keystore "D:\APP STITBD APP\glorious_sunday\credential\upload-keystore.jks" -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias upload
```

```bash
keytool -genkey -v -keystore "D:\APPSTITBD\PosApps\st_multi_pos\credential\upload-keystore.jks" -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias upload
```

---

## 📌 Step 2: Create `key.properties` File

Create a file named `key.properties` in the root of your Flutter project:

```properties
storePassword=123456
keyPassword=123456
keyAlias=upload
storeFile=..\\..\\credential\\upload-keystore.jks
```

---

## 📌 Step 3: Load Key Properties in `app/build.gradle`

Open `android/app/build.gradle` and add this at the top:

```groovy
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}
```

---

## 📌 Step 4: Configure Signing Configs

Inside `android/app/build.gradle`, update the `signingConfigs` and `buildTypes`:

```groovy
android {
    ...
    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
            storePassword keystoreProperties['storePassword']
        }
    }

    buildTypes {
        release {
            signingConfig signingConfigs.release
            shrinkResources true
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}
```

---

## 📌 Step 5: Build Signed App Bundle

Now run the following command to generate the `.aab` file:

```bash
flutter build appbundle
```

---

## ✅ Done!

Your Android app is now signed and ready for Play Store submission.  
Make sure to **keep your `key.properties` and keystore file safe and never upload them to GitHub**.
