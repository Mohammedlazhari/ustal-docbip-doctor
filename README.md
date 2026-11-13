# Ustal Docbip Doctor — Doctor Appointment Platform

**Flutter Production Application** for doctor-patient appointment management

**Author:** Mohammed Lazhari Krobba  
**Client:** Ustal.dev (Startup, Laghouat, Algeria)  
**Period:** February 2025  
**Tech Stack:** Flutter 3.x, Dart, GetX, Clean Architecture, Firebase, Google Maps

---

## 🎯 Highlights

- **Clean Architecture:** Model-View-Controller separation with GetX dependency injection
- **Multi-language Support:** Arabic, French, English with dynamic switching
- **Authentication:** JWT-based auth with secure token storage
- **Patient Management:** CRUD operations, search, filtering, pagination
- **Appointment System:** Booking, rescheduling, cancellation, reminders
- **Google Maps Integration:** Clinic locations, navigation, geofencing
- **Onboarding Flow:** Multi-step welcome, permission requests, tutorial
- **CI/CD Ready:** GitHub Actions for testing, building, deploying

---

## 🚀 Quick Start

```bash
git clone https://github.com/your-username/ustal-docbip-doctor.git
cd ustal-docbip-doctor

# Get dependencies
flutter pub get

# Run on device/emulator
flutter run -d <device-id>

# Build release
flutter build apk --release
flutter build ios --release
flutter build web --release
```

---

## 🏗 Architecture

### Clean Architecture with GetX

```
lib/
├── main.dart                          # App entry point
├── model/                             # Data layer
│   └── home_view_model.dart           # State management
├── view/                              # Presentation layer
│   ├── login/                         # Authentication screens
│   ├── SignUp/                        # Registration flow
│   ├── Onbording/                     # Welcome/tutorial
│   ├── HomeActions/                   # Main dashboard
│   ├── map/                           # Google Maps integration
│   ├── parametre/                     # Settings
│   ├── forget password/               # Password reset
│   └── ...
├── util/                              # Utilities
│   └── theme.dart                     # App theming
└── widgets/                           # Reusable components
```

### Key Components

| Component | Technology | Purpose |
|-----------|------------|---------|
| **State Management** | GetX | Reactive state, DI, routing |
| **Architecture** | Clean Architecture | Separation of concerns, testability |
| **Auth** | JWT + Secure Storage | Secure token management |
| **Database** | Firebase Firestore | Real-time sync, offline support |
| **Auth** | Firebase Auth | Email/password, social login |
| **Maps** | Google Maps SDK | Clinic locations, navigation |
| **Localization** | GetX Translations | Multi-language (AR/FR/EN) |
| **CI/CD** | GitHub Actions | Test, build, deploy |

### Dependency Injection (GetX)

```dart
// bindings/home_binding.dart
class HomeBinding extends Bindings {
  @override
  void dependencies() {
    Get.lazyPut<HomeController>(() => HomeController());
    Get.lazyPut<PatientRepository>(() => PatientRepositoryImpl());
    Get.lazyPut<AppointmentRepository>(() => AppointmentRepositoryImpl());
  }
}
```

---

## 🎯 Features

### Authentication Flow
- **Login/Signup:** Email/password with validation
- **Password Reset:** Email-based reset flow
- **JWT Tokens:** Secure storage with `flutter_secure_storage`
- **Auto-login:** Token refresh, session persistence
- **Multi-language:** Language selection at first launch

### Onboarding
- **Multi-step:** Welcome → Permissions → Tutorial → Language
- **Permission Handling:** Location, notifications, camera
- **Skip Option:** For returning users

### Home Dashboard
- **Patient List:** Search, filter, pagination, pull-to-refresh
- **Patient Details:** Medical history, appointments, notes
- **Quick Actions:** Call, message, navigate, add appointment

### Appointment Management
- **Booking:** Date/time picker, duration, clinic selection
- **Rescheduling:** Drag-drop or picker
- **Cancellation:** With reason, confirmation
- **Reminders:** Push notifications (FCM)

### Map Integration
- **Clinic Locations:** Markers with info windows
- **Navigation:** Google Maps directions
- **Geofencing:** Check-in/out automation
- **Offline Caching:** Map tiles for low connectivity

### Patient Management
- **CRUD:** Create, read, update, delete patients
- **Search:** Real-time, debounced, multi-field
- **Filters:** Status, date range, clinic
- **Export:** PDF reports, CSV export

---

## 🔧 Configuration

### Firebase Setup
```bash
# 1. Create Firebase project
# 2. Add Android/iOS/Web apps
# 3. Download google-services.json / GoogleService-Info.plist
# 4. Enable: Authentication, Firestore, FCM, Storage
# 5. Configure Firestore rules
```

### Google Maps API
```yaml
# pubspec.yaml
dependencies:
  google_maps_flutter: ^2.5.0
  geolocator: ^10.1.0

# Android: android/app/src/main/AndroidManifest.xml
<meta-data android:name="com.google.android.geo.API_KEY" android:value="YOUR_API_KEY"/>

# iOS: ios/Runner/AppDelegate.swift
GMSServices.provideAPIKey("YOUR_API_KEY")
```

### Localization
```dart
// util/translations.dart
class AppTranslations extends Translations {
  @override
  Map<String, Map<String, String>> get keys => {
    'en_US': {'login': 'Login', 'patient': 'Patient', ...},
    'fr_FR': {'login': 'Connexion', 'patient': 'Patient', ...},
    'ar_AR': {'login': 'تسجيل الدخول', 'patient': 'مريض', ...},
  };
}
```

---

## 🧪 Testing

```bash
# Unit tests
flutter test

# Widget tests
flutter test test/widget_test.dart

# Integration tests
flutter test integration_test/

# Coverage
flutter test --coverage
genhtml coverage/lcov.info -o coverage/html
```

---

## 🏗 Build & Release

```bash
# Android APK
flutter build apk --release --split-per-abi
# Output: build/app/outputs/flutter-apk/app-[abi]-release.apk

# Android App Bundle (Play Store)
flutter build appbundle --release
# Output: build/app/outputs/bundle/release/app-release.aab

# iOS
flutter build ios --release --no-codesign
# Then open in Xcode for signing & archive

# Web
flutter build web --release
# Output: build/web/
```

### CI/CD (GitHub Actions)

```yaml
# .github/workflows/flutter-ci.yml
name: CI/CD
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: subosito/flutter-action@v2
        with:
          flutter-version: '3.19.0'
      - run: flutter pub get
      - run: flutter analyze
      - run: flutter test --coverage
      - uses: codecov/codecov-action@v3

  build-android:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: subosito/flutter-action@v2
      - run: flutter pub get
      - run: flutter build apk --release --split-per-abi
      - uses: actions/upload-artifact@v4
        with:
          name: apk
          path: build/app/outputs/flutter-apk/

  build-ios:
    needs: test
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v4
      - uses: subosito/flutter-action@v2
      - run: flutter pub get
      - run: flutter build ios --release --no-codesign
      - uses: actions/upload-artifact@v4
        with:
          name: ios
          path: build/ios/iphoneos/
```

---

## 📸 Screenshots

| Screen | Description |
|--------|-------------|
| ![Login](screenshots/login.png) | Multi-language login with validation |
| ![Onboarding](screenshots/onboarding.png) | 4-step welcome flow |
| ![Dashboard](screenshots/dashboard.png) | Patient list with search, filter |
| ![Map](screenshots/map.png) | Clinic locations with navigation |
| ![Appointment](screenshots/appointment.png) | Booking flow with calendar |
| ![Profile](screenshots/profile.png) | Settings, language, logout |

---

## 📦 Dependencies

```yaml
dependencies:
  flutter:
    sdk: flutter
  get: ^4.6.6                    # State management, DI, routing
  firebase_core: ^2.24.0         # Firebase init
  firebase_auth: ^4.15.0         # Authentication
  cloud_firestore: ^4.13.0       # Database
  firebase_messaging: ^14.7.0    # Push notifications
  google_maps_flutter: ^2.5.0    # Maps
  geolocator: ^10.1.0            # Location
  flutter_secure_storage: ^9.0.0 # Secure token storage
  intl: ^0.19.0                  # Localization
  image_picker: ^1.0.0           # Camera/gallery
  permission_handler: ^11.0.0    # Permissions
  url_launcher: ^6.2.0           # External links
  flutter_local_notifications: ^16.0.0 # Local notifications
```

---

## 📄 License

MIT License — see LICENSE file.