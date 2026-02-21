# 🐻 Interactive Animated Login | Flutter & Rive

An engaging and interactive login screen built with Flutter, featuring a dynamic bear character that reacts to user input in real-time using Rive animations and State Machines.

## 🚀 Key Functionalities

This project demonstrates a high-level integration between UI components and vector animations:

- 👀 **Dynamic Gaze:** The bear follows the cursor and tracks the text length while the user is typing in the Email field using the `isChecking` input.
- 🙈 **Privacy Mode:** When the Password field is focused or tapped, the bear covers its eyes using the `isHandsUp` boolean state.
- 👁️ **Peek Feature:** Toggling the password visibility (eye icon) programmatically lowers the bear's hands so it can "peek" while the user checks their password.
- ⚡ **State Machine Management:** Real-time control of animations via `StateMachineController`, connecting Flutter's `FocusNodes` with Rive's internal logic.
- 🛠️ **Memory Management:** Efficient handling of resources by disposing of `FocusNodes` to prevent memory leaks.

## 🎨 What is Rive & State Machine?

### Rive

Rive is a real-time interactive design and animation tool. Unlike traditional video or GIF formats, Rive animations are functional code, allowing them to be manipulated dynamically, resulting in tiny file sizes and high performance.

### State Machine

The State Machine is the "brain" of the animation. In this project, the `Login Machine` manages:

- **SMI (State Machine Inputs):** Variables like `isChecking` (bool), `isHandsUp` (bool), `trigSuccess` (trigger), and `trigFail` (trigger) that allow the Flutter code to change the animation state instantly.

## 🛠️ Technologies

- **Framework:** Flutter 🚀
- **Language:** Dart 🎯
- **Animation Engine:** Rive Runtime 🎬

## 📂 Project Structure

The main logic is organized as follows:

```
lib/
├── main.dart              # Entry point of the app
└── login_screen.dart      # Main UI and State Machine logic
assets/
└── animated_login_bear.riv # Teddy's animation source file
```

## 🎬 Demo

> Note: Upload a GIF of your app to your repository and replace the link above to show the animation in action!

## 🎓 Academic Information

- **Subject:** \[Insert Subject Name Here\] 📚
- **Professor:** \[Insert Professor Name Here\] 👨‍🏫

## 📜 Credits

The character animation used in this project is the famous Teddy from the Rive community.

- **Original Creator:** Rive Community - Teddy 👏

## ⚙️ How to use

1. Add the `rive` dependency to your `pubspec.yaml`.
2. Ensure `animated_login_bear.riv` is declared in your assets section.
3. Run `flutter pub get`.
4. Run the project on your emulator or physical device.
