# [  Expo ]

## Install Expo

*From: https://docs.expo.io/get-started/installation/*

### [Requirements](https://docs.expo.io/get-started/installation/#requirements)

- [Node.js LTS release](https://nodejs.org/en/) or greater
- [Git](https://git-scm.com)
- [Watchman](https://facebook.github.io/watchman/docs/install#buildinstall) for macOS users

### [Recommended Tools](https://docs.expo.io/get-started/installation/#recommended-tools)

- [VSCode Editor](https://code.visualstudio.com/download)
- [Yarn](https://classic.yarnpkg.com/en/docs/install)
- Windows users: [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-windows) or Bash via WSL

### [Installing Expo CLI](https://docs.expo.io/get-started/installation/#installing-expo-cli)

```
# Install the command line tools
npm install --global expo-cli
```

Verify that the installation was successful by running 

```bash
expo whoami
----
› Not logged in, run expo login to authenticate
----
```

You're not logged in yet, so you will see "Not logged in". You can create an account by running `expo register` if you like, or if you have one already run `expo login`, but you also don't need an account to get started.

> 😳
>
>  **Need help?** Try searching the [forums](https://forums.expo.io) — which are a great resource for troubleshooting.



## [2. Expo Go app for iOS and Android](https://docs.expo.io/get-started/installation/#2-expo-go-app-for-ios-and)

The fastest way to get up and running is to use the Expo Go app on your iOS or Android device. Expo Go allows you to open up apps that are being  served through Expo CLI.

- 🤖 [Android Play Store](https://play.google.com/store/apps/details?id=host.exp.exponent) - Android Lollipop (5) and greater.
- 🍎 [iOS App Store](https://itunes.com/apps/exponent) - iOS 11 and greater.

When the Expo Go app is finished installing, open it up. If you created an account with `expo-cli` then you can sign in here on the "Profile" tab. This will make it  easier for you to open projects in the client when you have them open in development — they will appear automatically in the "Projects" tab of the client app.

> 👉
>
>  It's often useful to be able to run your app directly on your computer  instead of on a separate physical device. If you would like to set this  up, you can learn more about [installing the iOS Simulator (macOS only)](https://docs.expo.io/workflow/ios-simulator/) and [installing an Android emulator](https://docs.expo.io/workflow/android-studio-emulator/).

## 