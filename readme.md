# 📱 BBM Indo - Android Production Build Guide

This guide covers the 2026 build requirements for BBM Indo,
targeting API Level 35 (Android 15) while handling modern
library conflicts.

## 🛠 Required Environment

Your machine must match this environment:

* Java JDK: Version 21 (Required for AGP 8+)
* Android SDK Platform: API 35 (Target) and API 36 (Compile)
* Build-Tools: 35.0.0 or 36.0.0
* Node.js: 20.x or 22.x

---

## ⚙️ Configuration Summary

### 1. SDK Versioning (android/variables.gradle)
We use a "Split SDK" strategy to satisfy new libraries
while maintaining stable behavior.

- minSdkVersion: 24
- compileSdkVersion: 36
- targetSdkVersion: 35
- buildToolsVersion: "35.0.0"

### 2. Java Pathing (android/gradle.properties)
Force Java 21 to prevent "invalid source release" errors:

org.gradle.java.home=/opt/homebrew/opt/openjdk@21/libexec/openjdk.jdk/Contents/Home

### 3. Dependency Pinning (android/app/build.gradle)
Forces libraries to stay on API 35-compatible versions:

configurations.all {
    resolutionStrategy {
        force "androidx.core:core:1.15.0"
        force "androidx.core:core-ktx:1.15.0"
        force "androidx.activity:activity:1.10.1"
    }
}

---

## 🚀 Build Instructions

Run these commands in order:

1. Sync Assets:
   npm run build
   npx cap sync android

2. Set Java 21 in Terminal:
   export JAVA_HOME=$(/usr/libexec/java_home -v 21)

3. Generate Release AAB:
   cd android
   ./gradlew clean
   ./gradlew bundleRelease --no-daemon

---

## 📦 Output Location

Final signed bundle:
android/app/build/outputs/bundle/release/app-release.aab

---

## ⚠️ Troubleshooting

* Error: invalid source release: 21
  Fix: Run export JAVA_HOME command in step 2.

* Error: core:1.17.0 requires API 36
  Fix: Ensure compileSdkVersion is 36 in variables.gradle.

* Error: Version Code 13 already exists
  Fix: Bump versionCode to 14 in app/build.gradle.