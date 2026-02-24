[DEPRECATED]
# Android Build Instructions

## 1. Set up Android SDK (First Time Only)

1. Download Android SDK Command Line Tools from the official site.
2. Extract to: `~/Library/Android/sdk/cmdline-tools/latest`
3. Add to your `~/.zshrc`:
	```sh
	export ANDROID_HOME=$HOME/Library/Android/sdk
	export PATH=$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools:$PATH
	```
	Then run: `source ~/.zshrc`
4. Install required SDK packages:
	```sh
	sdkmanager --sdk_root=$ANDROID_HOME "platform-tools" "platforms;android-33" "build-tools;33.0.0"
	```

## 2. Build the App Bundle

1. Change `versionCode` and `versionName` in `android/app/build.gradle` as needed.
2. Run:
	```sh
	cd android
	./gradlew build bundle
	```

The output `.aab` file will be at:
`android/app/build/outputs/bundle/release/app-release.aab`

# Development
- see `package.json`