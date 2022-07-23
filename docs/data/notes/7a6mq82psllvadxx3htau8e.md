
## Android
1. need an Android SDK (download in SDK Manager of Android Studio)
2. need a JDK installation
```
brew tap AdoptOpenJDK/openjdk
brew cask install adoptopenjdk8
```
- check installation success `java -version`

3. set environment variables (verify Android SDK location)
```sh
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

4. Ensure SDK Manager (Android Studio) is pointing to correct path
5. run `adb` and verify the ANDROID_HOME directory is pointed to.