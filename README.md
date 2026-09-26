# NomMetric
A Flutter-based mess tracking app designed for college use, enabling students to track meals across multiple messes, developed for OpenCode’25.

<p align="left">
  <a href="agent.yaml"><img src="https://img.shields.io/badge/OpenGAP-0.1.0-blue.svg" alt="OpenGAP Spec"></a>
  <a href="https://app.hidevs.xyz/passport/submit"><img src="https://img.shields.io/badge/GitAgent%20Passport-Ready-green.svg" alt="GitAgent Passport Ready"></a>
  <a href="agent.yaml"><img src="https://img.shields.io/badge/Category-Education-purple.svg" alt="Category Education"></a>
  <a href="EXPLAINABILITY.md"><img src="https://img.shields.io/badge/Compliance-FERPA%20%7C%20GDPR-orange.svg" alt="Compliance FERPA | GDPR"></a>
</p>

## Project Overview

NomMetric is a Flutter-based mess tracking application designed to simplify and digitize meal management in our college hostels.
In many colleges, mess attendance, meal tracking, and rebate calculations are handled manually, leading to inaccuracies, lack of transparency, and poor user experience.

NomMetric aims to provide a unified, scalable solution where students can track their meals across multiple messes, while enabling accurate attendance logging and data-driven decision-making.

The application is built using Flutter for cross-platform support and Firebase (Firestore) as the backend, ensuring real-time updates, reliability, and ease of scalability.
In addition to being a practical utility, NomMetric is developed as a learning-focused open-source project, allowing contributors to gain hands-on experience with Flutter, Firebase, and real-world application architecture.
### Core Features

#### 1. Meal & Attendance Tracking
- Track student attendance for each meal (breakfast, lunch, dinner)
- Support for multiple messes within the same campus
- Accurate daily and monthly attendance records
- Reduced ambiguity compared to manual attendance systems

#### 2. Rebate Calculation System
- Automatic calculation of mess rebates based on missed meals
- Monthly rebate summaries for students
- Configurable rebate rules to match different colleges
- Transparent rebate history to avoid disputes

#### 3. Menu Management
- Digital mess menu visible to students
- Support for daily and weekly menus
- Easy updates to menu items without app redeployment
- Support for special meals and holiday menus

#### 4. Data Persistence & Real-time Sync
- Firestore-backed real-time data updates
- Consistent data across devices and sessions
- Offline-friendly architecture with automatic sync when online

#### 5. Clean & Modular UI
- Simple, intuitive interface designed for daily use
- Modular widget structure for easy feature expansion
- Responsive layouts for different screen sizes

---

NomMetric is intended to be both a **practical campus utility** and a **learning-focused open-source project**, making it suitable for experimentation, feature expansion, and architectural improvements.


## Quick Start
Ready to contribute? Check out our [Contributing Guide](Contributing.md). It covers everything you need to know about setting up the project, claiming issues, and submitting pull requests for OpenCode'25.

### Prerequisites
Before you begin working on this project, ensure your environment meets the following requirements.

#### Core Tooling (All OS)
- **Flutter SDK**: `^3.27.0`
- **Dart SDK**: `3.6.0` (bundled with Flutter 3.27.x)
- **Java Development Kit (JDK)**: `17`(recommended) or `21`

#### Platform-Specific Tooling
- **Android SDK** (Linux / macOS / Windows):
  - Compile SDK / Target SDK: `API level 35` (Android 15, recommended)
- **iOS SDK** (macOS only):
  - Requires Xcode and iOS platform tools

#### OS Compatibility
This project uses Flutter and Firebase and is **largely OS-independent**.

- The setup instructions apply to **Linux, macOS, and Windows**.
- Android development is supported on all three operating systems.
- **iOS and macOS builds require macOS** with Xcode installed.
- Installation steps for the Android SDK and JDK may differ slightly by OS, but the required versions remain the same.


#### Required Flutter Packages

These following packages are used in this project:
- **flutter_riverpod**: `^2.5.1`  
  https://pub.dev/packages/flutter_riverpod

- **firebase_core**: `^3.6.0`  
  https://pub.dev/packages/firebase_core

- **cloud_firestore**: `^5.4.0`  
  https://pub.dev/packages/cloud_firestore

- **go_router**: `^14.2.0`  
  https://pub.dev/packages/go_router

### Notes
- Ensure `compileSdkVersion` and `targetSdkVersion` are set to `35`.
- Run `flutter doctor` to verify that all dependencies are correctly installed.

Try checking out [contributing guide](Contributing.md) for further help.

## Learning Resources

If you’re new to Flutter, Firebase, or Riverpod, the resources below will help you get started and contribute effectively.

### Flutter
- Flutter Documentation: https://docs.flutter.dev
- Flutter Widget Catalog: https://docs.flutter.dev/ui/widgets
- Flutter YouTube (official): https://www.youtube.com/@flutterdev

### Firebase & Firestore
- Firebase for Flutter: https://firebase.flutter.dev
- Firestore Documentation: https://firebase.google.com/docs/firestore
- Firestore Data Modeling (video): https://www.youtube.com/watch?v=v_hR4K4auoQ

### State Management (Riverpod)
- Riverpod Documentation: https://riverpod.dev
- Riverpod Crash Course (video): https://www.youtube.com/watch?v=3OdciTLjS8k

### Project-Specific References
- FlutterFire CLI: https://firebase.flutter.dev/docs/cli
- GoRouter Documentation: https://pub.dev/packages/go_router


## Setting up
1. Clone the repo
``` bash
git clone https://github.com/opencodeiiita/NomMetric.git
cd NomMetric
```

2. Link the repository to your Firebase project using the application ID defined in `android/app/build.gradle`.

Current applicationID : `com.example.nommetric`


## Project Structure
```text
NomMetric/
├── android/           # Android native configurations
├── ios/               # iOS native configurations
├── web/               # Web platform support
├── assets/            # Images, fonts, and local data files
├── lib/               # Primary application code
│   ├── screens/       # Full-page UI components
│   ├── widgets/       # Reusable UI elements
│   ├── services/      # Logic for Firebase, Firestore, and APIs
│   ├── models/        # Data structures; (User, Meal, Rating objects)
│   ├── provider/      # State management logic (Riverpod)2
├── contributors.md    # Registration file for participants
└── pubspec.yaml       # Project dependencies and configuration
```

---

## 🤖 GitAgent Passport Qualification

This repository is compliant with the **OpenGAP Spec 0.1.0** standard and qualified for the [HiDevs GitAgent Passport](https://app.hidevs.xyz/passport/submit).

### Clearance Checkpoints Summary

| Checkpoint | Status | Focus Area | Artifact |
| :--- | :--- | :--- | :--- |
| **Checkpoint 1: Validate** | `PASSED` | Schema, Soul, Skills, Tools | [`agent.yaml`](agent.yaml), [`SOUL.md`](SOUL.md), [`skills/`](skills/), [`tools/`](tools/) |
| **Checkpoint 2: Explain** | `PASSED` | Decision Logic, Data Usage, Limitations | [`EXPLAINABILITY.md`](EXPLAINABILITY.md) |
| **Checkpoint 3: Export** | `PASSED` | Interoperability & Tool Schemas | [`tools/`](tools/), [`RULES.md`](RULES.md), [`DUTIES.md`](DUTIES.md) |

### Agent Architecture Overview

- **Identity & Ethics**: [`SOUL.md`](SOUL.md) defines core behavioral traits, dining governance principles, and student fairness commitments.
- **Operational Rules**: [`RULES.md`](RULES.md) establishes absolute constraints on duplicate check-in prohibition, rebate notice windows, and FERPA/GDPR compliance.
- **Role Duties**: [`DUTIES.md`](DUTIES.md) defines step-by-step responsibilities across daily meal session orchestration, rebate auditing, menu coordination, and monthly reconciliation.
- **Transparent Reasoning**: [`EXPLAINABILITY.md`](EXPLAINABILITY.md) documents step-by-step rationale, mathematical rebate formulas, data provenance, and clear operational boundaries.
- **Modular Skills**: Located in [`skills/`](skills/) for meal attendance tracking, rebate auditing, menu coordination, and dining analytics forecasting.
- **Tool Schemas**: Standardized JSON schemas located in [`tools/`](tools/) for attendance logging, rebate calculations, menu publication, and dining reporting.

---

## Contributing

We welcome contributions as part of OpenCode’25.

- Browse the repository issues and pick one that matches your interest and skill level.
- Issues may be:
  - **Open-for-all (OFA)**: No claiming required; anyone can submit a PR.
  - **Competitive**: Multiple submissions allowed; the best PR is accepted.
  - **First-come**: Must be claimed before working on it and has a time limit.

To contribute:
1. Fork the repository and create a new branch.
2. Make your changes following the project guidelines.
3. Add your details to [contributors.md](contributors.md).
4. Open a pull request with a clear title and link it to the relevant issue.

For detailed rules, issue claiming process, and PR requirements, refer to  
[Contributing.md](Contributing.md).


## Contact

If you have any questions, run into issues while setting up, or need clarification on contributions, feel free to reach out.

- **Discord**: `VirtualVard`

You can also contact me for reporting bugs, feature requests, or suggestions about this repository.

---

Happy contributing, and welcome to NomMetric!
