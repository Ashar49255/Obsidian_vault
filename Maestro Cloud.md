https://github.com/obytes/react-native-template-obytes

Maestro is the simplest and most effective framework for painless mobile and web UI automation using intuitive YAML flows.

[Maestro Cloud](https://www.google.com/search?q=Maestro+Cloud&oq=what+is+maestro+cloud+and+how+it+works+&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIHCAEQIRigATIHCAIQIRigAdIBCDk5NjZqMGo3qAIIsAIB8QUtkUClIQyNHPEFLZFApSEMjRw&sourceid=chrome&ie=UTF-8&mstk=AUtExfBn4NgXz76t6VWMSVa02IF_2iQFzuHuxhDdRlynadKMyPjwCvcKMX07GDwxt9foV8oOo4e6glH3Dg8qYm-QV1vRPticnEhkx1_IkCM8_ABWl6h0nhniyMjEMIAF6LalM9TDggED4_SHh-AoeLiO28pxYCp0XhMTeAG2gbikHWTKii0fmG8iT0N5oXh9GXV8cSGkkEcaieM806N__UD9OhKvCxU9lpJJFeCwSsDnsJzg-jSHo5npSo6fB3I2S3I2woMa5ytoFrE1BZKSzP4k9mvJ&csui=3&ved=2ahUKEwj2672NleOTAxUnVqQEHSVBL3wQgK4QegQIARAB) is a managed, cloud-based platform for mobile UI testing that allows developers to run automated Maestro tests on iOS and Android apps without maintaining local simulators or devices. It enables parallel testing, providing faster, scalable, and reliable end-to-end (E2E) testing that integrates into CI/CD pipelines. 

It is used to **automatically test apps** (Android, iOS, React Native, etc.).

> Maestro = tool that behaves like a real user and tests your app automatically


## Why choose Maestro?

**Built-in Tolerance**

Maestro embraces the instability of mobile devices by automatically handling flakiness and UI settling.

**Zero-Wait Intelligence**

No more manual `sleep()` calls. Maestro automatically waits for network content and animations to load.

**Declarative Syntax**

Tests are defined in human-readable YAML files, removing the need for deep programming knowledge.

**Blazingly Fast Iteration**

Tests run without compilation. Maestro can monitor your files and rerun flows instantly upon saving.

**Single Binary Setup**

Maestro is a single tool that works anywhere, avoiding the "setup hell" associated with legacy drivers.

**Maestro is a single platform that tests all of your mobile and web apps — no matter what framework your team uses. [Maestro](https://maestro.dev/) It supports: iOS, Android, Web, React Native, Flutter, Flutter Web, Mobile Browser, Web Views, Jetpack Compose, SwiftUI, NativeScript, .NET MAUI, Capacitor, and Cordova.**

## 🔄 CI/CD Integration (Shift-Left Testing)

With Maestro, catching issues early in the development lifecycle is dead simple. You can protect every workflow so you find problems before your users do — integrating into CI for pre-release checks, nightly runs, and pull request validation

-----------

## Maestro CLI
The CLI is the open-source engine that interprets YAML files and sends instructions to the device. It is the primary tool for automated execution in Continuous Integration (CI) systems.


## Maestro Cloud
Maestro Cloud acts as the execution backend for large-scale testing. Teams upload their apps and Flows to run in parallel across multiple hosted virtual devices, ensuring deterministic results and faster feedback loops.


--------------------------
cmd:
device detect check
adb devices

Expo App
```
The Expo app (or Expo framework) isa  popular open-source, full-stack platform used to build native Android, iOS, and web applications using React Native and JavaScript/TypeScript==. It streamlines the development lifecycle by abstracting complex native code, offering pre-built SDK libraries, and allowing developers to create apps without needing macOS or Android Studio
```


```
# React Native App create (Recommended: Expo)


### Install Expo CLI

npm install -g create-expo-app

### new project create 

npx create-expo-app myApp  
cd myApp

### App run 

npx expo start

---

# 📱 2. Emulator setup (Android)

### Android Studio install karo


### Expo app run on emulator:

npx expo run:android


adb devices

---

# 🧪 3. Maestro install ()

👉 Maestro

### Install:

curl -Ls "https://get.maestro.mobile.dev" | bash

Check:

maestro --version
```





```
cd react-native-template-obytes
pnpm install
pnpm start




```

---------------------------------------

** 3 types of pipelines in Repo :**
### 1.  Testing
- e2e-android
- maestro
- test.yml
### 2. Build
- eas-build-preview
- eas-build-prod
- eas-build (action)
### 3. Quality / Automation
- lint
- type-check
- expo-doctor
- compress-images
- stale
  
  
  **safety checks** :

| Check      | Purpose        |
| ---------- | -------------- |
| Lint       | clean code     |
| Type-check | bug prevention |
| Tests      | functionality  |

## expo-doctor
`check karta hai:
 dependencies sahi hain ya nahi
 Expo SDK compatible hai ya nahi
 versions match kar rahi hain ya nahi`
 commit -m"thsi isss"