# 🐻 Animated Login with Rive — Flutter

> A delightful, interactive login screen powered by a Rive state machine and a charming animated bear character. Watch him track your email input, cover his eyes when you type your password, and react to login success or failure — all in real time. ✨

---

## ✨ Features

- 👀 **Gaze Tracking** — The bear follows your cursor as you type in the email field, simulating real eye movement tied to text input.
- 🙈 **Privacy Mode** — The bear covers his eyes when the password field is focused, and peeks when you toggle password visibility.
- ✅❌ **Visual Validation** — Trigger-based animations react to login success or failure, giving instant expressive feedback.

---

## 🧠 Key Concepts

### What is Rive?
[Rive](https://rive.app) is a real-time interactive animation tool that lets designers and developers create vector animations that respond to state, data, and user interaction. Unlike traditional GIF or Lottie animations, Rive animations are fully interactive and can be controlled programmatically at runtime.

### What is a State Machine?
A **State Machine** in Rive is a visual logic layer built on top of animations. It defines a set of **states** (e.g., `Idle`, `Checking`, `HandsUp`, `Success`, `Fail`) and **transitions** between them, driven by **inputs** — booleans, numbers, or triggers — that your app controls at runtime. This allows complex, branching animation behavior without writing animation code from scratch.

---

## 🛠️ Tech Stack

| Technology | Role |
|------------|------|
| 🐦 **Flutter** | UI framework and widget tree |
| 🎯 **Dart** | Programming language |
| 🎨 **Rive** | Real-time animation & state machine engine |

---

## 🎮 State Machine Inputs

This project controls a Rive animation called **`Login Machine`** via `StateMachineController` with the following inputs:

```dart
// Booleans
isChecking  // true → bear tracks the email text field
isHandsUp   // true → bear covers eyes (password field focused)

// Triggers
trigSuccess // fires on successful login
trigFail    // fires on failed login attempt
```

The `obscureText` toggle on the password field is synced with `isHandsUp`, enabling a **"peek" mode** where the bear briefly uncovers his eyes when the user reveals their password.

---

## 📁 Project Structure

```
flutter_rive_login/
│
├── lib/
│   ├── main.dart               # App entry point
│   └── login_screen.dart       # Main login UI + Rive controller logic
│
├── assets/
│   └── animations/
│       └── login_teddy.riv     # Rive animation file (bear character)
│
├── pubspec.yaml                # Dependencies & asset declarations
└── README.md
```

---

## ⚙️ Getting Started

### Prerequisites

- Flutter SDK `>=3.0.0`
- Dart `>=3.0.0`

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/flutter-rive-login.git
cd flutter-rive-login

# 2. Install dependencies
flutter pub get

# 3. Run the app
flutter run
```

### Dependencies (`pubspec.yaml`)

```yaml
dependencies:
  flutter:
    sdk: flutter
  rive: ^0.12.0   # Check pub.dev for the latest version
```

Don't forget to declare your assets:

```yaml
flutter:
  assets:
    - assets/animations/login_teddy.riv
```

---

## 🧩 Core Implementation

### Initializing the State Machine

```dart
StateMachineController? _controller;
SMIBool? isChecking;
SMIBool? isHandsUp;
SMITrigger? trigSuccess;
SMITrigger? trigFail;

void _onRiveInit(Artboard artboard) {
  final controller = StateMachineController.fromArtboard(
    artboard,
    'Login Machine',
  );
  if (controller != null) {
    artboard.addController(controller);
    isChecking  = controller.findInput<bool>('isChecking')  as SMIBool;
    isHandsUp   = controller.findInput<bool>('isHandsUp')   as SMIBool;
    trigSuccess = controller.findInput<bool>('trigSuccess') as SMITrigger;
    trigFail    = controller.findInput<bool>('trigFail')    as SMITrigger;
  }
}
```

### FocusNode Listeners

```dart
_emailFocusNode.addListener(() {
  isChecking?.change(_emailFocusNode.hasFocus);
});

_passwordFocusNode.addListener(() {
  isHandsUp?.change(_passwordFocusNode.hasFocus);
});
```

---

## 🎬 Demo

> 📸 *Replace this section with a GIF or screen recording of the app in action.*

```
[  Insert demo GIF here  ]
```

*To record a demo: run the app on an emulator, use a screen recorder, and convert to GIF using a tool like [ezgif.com](https://ezgif.com) or `ffmpeg`.*

---

## 🎓 Academic Info

| Field | Details |
|-------|---------|
| 📚 **Subject Name** | `[ Subject Name Here ]` |
| 👨‍🏫 **Professor Name** | `[ Professor Name Here ]` |
| 🏫 **Institution** | `[ Institution Name Here ]` |
| 📅 **Semester / Year** | `[ e.g., Spring 2025 ]` |

---

## 🙏 Credits

The animated bear (Teddy) character used in this project was originally created for the **Rive Community** and deserves full recognition:

> 🎨 **Original Animation:** [Teddy Login Screen](https://rive.app/community/files/1563-3098-animated-login-screen/) by **JcToon** on the Rive Community.

Please visit the original file and give the creator a ❤️ if you use or are inspired by this animation.

---

## 📄 License

This project is intended for **educational purposes**. The Rive animation asset is subject to the original creator's licensing terms on the Rive Community platform. Please review those terms before using it in commercial projects.

---

<p align="center">Made with ❤️ and Flutter 🐦</p>
