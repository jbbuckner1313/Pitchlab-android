# PitchLab Focus

Android pitching interface with a PitchLogic-style Focus dashboard, History, Settings, and an illustrative pitch replay. This is an original app, not the official PitchLogic app.

## Version 0.5: connection and pitch displays

- Tap **Connect ball** to scan for and automatically select a compatible PitchLogic ball. The app verifies the PitchLogic data service before enabling the live notification stream.
- The connection button becomes **Cancel search** while scanning and **Disconnect** while connecting or connected. Failed connections time out and can be retried.
- Unrelated BLE devices are excluded, stale callbacks are ignored, and scanning stops when a ball is selected. Successful ball addresses are remembered.
- Battery notifications update the connection indicator.
- The Focus screen includes velocity, spin, spin-axis and spin-direction displays, horizontal/vertical movement, and editable pitch tags.
- The ball display supports drag rotation, pinch zoom, and up to five user-marked last-touch points per saved pitch. Seam geometry is illustrative. Annotations are not sensor-measured last-touch readings.
- Comparison values and pitch tags can be edited and saved. History opens individual pitch details. Manual pitch records and details can be shared.
- Bluetooth diagnostics are available in Settings.

## Sensor decoding status

The previously implemented spin decoder was invalid: timer-like fields could be misreported as RPM, including the reported 318 rpm reading. Automatic metric decoding and automatic pitch saving are disabled pending validation of the packet layout against known readings.

A Bluetooth connection and incoming notifications do not establish accurate velocity, spin, axis, direction, movement, pitch classification, or last-touch measurements. The current measurement displays use manual entries; unknown sensor metrics remain blank. Pitch labels are user tags, not automatic classifications.

## Install and use

This build uses application ID `com.example.pitchlab.focus`, displayed as **PitchLab Focus**, version code 5. It installs alongside the older PitchLab BLE Fix app. Updates signed with the same key preserve Focus's saved records.

1. Close nRF Connect, the official PitchLogic app, and older PitchLab apps so the ball is free to connect.
2. Wake the ball, keep it near the phone, and tap Connect ball. Grant Nearby devices permission and enable Bluetooth if prompted.
3. Confirm the ball connection. To release it for another app, tap Disconnect.
4. Until live decoding is validated, add a manual pitch in Settings. Open its details from History to edit the pitch label and comparison metrics or mark last-touch annotations.

## Build

The repository's GitHub Actions workflow extracts `PitchLab-android-source.zip`, builds the Android project, and uploads the APK artifact. For local builds, extract the archive and run Gradle 8.10.2 with JDK 17 and Android SDK 35:

```
gradle :app:assembleDebug --no-daemon
```

The project uses Android Gradle Plugin 8.7.3 and Kotlin 2.0.21, targets Android 35, and supports Android 8 or later. APK compilation and signing verification passed locally. Physical phone BLE testing and live-metric validation remain outstanding. The replay path and displayed seams are illustrative.
