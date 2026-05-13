# Flutter Handoff

Created by: Flutter Agent
Phase: 3
Inputs: selected release scope, API contract, design spec, DEV handoff when web
backend already exists

## Scope Completed

- Built MVP 1 Flutter app under `project_code/mobile/`.
- Implemented login, registration, verification result, and dashboard screens.
- Connected mobile client to the documented API contract.
- Added unit, widget, and integration test coverage for primary flows.

## Generated Paths

- App root: `project_code/mobile/`
- Source: `project_code/mobile/lib/`
- Unit/widget tests: `project_code/mobile/test/`
- Integration tests: `project_code/mobile/integration_test/`
- Android project: `project_code/mobile/android/`
- iOS project: `project_code/mobile/ios/`

## Install Commands

```bash
cd project_code/mobile && flutter pub get
```

## Run Commands

```bash
cd project_code/mobile && flutter run
```

Expected runtime dependency:

- API base URL from client-safe env/config value.

## Test Commands

```bash
cd project_code/mobile && flutter analyze
cd project_code/mobile && flutter test
cd project_code/mobile && flutter test integration_test/
```

## Build Commands

Android:

```bash
cd project_code/mobile && flutter build apk
```

iOS:

- Local IPA build was not attempted on Windows/Linux.
- iOS release build requires macOS, Xcode, Apple signing material, and DevOps
  CI setup.

## Artifact Paths

- Android APK: `project_code/mobile/build/app/outputs/flutter-apk/app-release.apk`
- Android AAB: not built unless Play Store distribution is selected.
- iOS IPA: not built locally.

## Required Env Var Names

- `API_BASE_URL`
- `ANDROID_PACKAGE`
- `BUNDLE_ID`
- `APPLE_TEAM_ID`
- `IOS_DISTRIBUTION_CHANNEL`
- `ANDROID_DISTRIBUTION_CHANNEL`

Mobile clients must not use server-only secrets.

## Verification Results

- `flutter pub get`: pass.
- `flutter analyze`: pass.
- `flutter test`: pass.
- `flutter test integration_test/`: pass against local API target.
- `flutter build apk`: pass when Android is in scope.

## Platform Caveats

- iOS signing material is not stored in `.env` or committed files.
- First TestFlight or Play Console upload may require user account agreements.
- DevOps owns distribution workflows after explicit approval.

## Handoff

Mobile implementation is ready for Code Review and QA. If QA finds mobile
failures, Team Lead should assign fixes to Flutter unless the issue is API
contract, backend behavior, or external account configuration.
