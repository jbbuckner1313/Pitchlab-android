# PitchLab Android — PitchLogic live decoding build

Open the project in Android Studio, let Gradle sync, and run it on a physical Android device (Android 8 or later). Bluetooth Low Energy cannot be tested in the usual emulator.

## Features ready now

- Find nearby Bluetooth LE devices, connect to one, discover its GATT services and characteristics, and subscribe to notification and indication characteristics one at a time.
- Timestamp notifications as hex, save the trace locally, and share the trace through Android's share menu.
- Decode the tested PitchLogic `4b810002` notification stream and automatically save velocity and spin for each pitch event.
- Enter pitch type, speed and spin manually; review pitches and share them as CSV. CSV labels these rows `manual`.

## Test with the ball

1. Charge the ball, turn on Bluetooth, open PitchLab, grant nearby device permission, and tap **Find ball / BLE devices**.
2. Tap the ball's device entry and wait for `Connected` and `Service` entries. If no entry appears, check that the official app is closed and the ball is awake.
3. Select the pitch type, then make a safe test throw with the phone out of the throwing area. PitchLab will pair the speed and spin summaries by event number and add one entry automatically.
4. Compare several readings with the official app during final calibration. Use **Share Bluetooth diagnostics** if a pitch is missed.

## Implemented packet map

- Characteristic: `4b810002-0394-7ff1-f00b-ab59cb8a9de3`
- Velocity: packet `9A-1C-50-[event]`, little-endian float32 at byte offset 26, mph.
- Spin: packet `9A/9E-1B-68-[event]`, median of the repeated valid RPM summary fields.
- Calibration/configuration constants such as the repeated `25.9527` value are not treated as velocity.
- Values outside 10–130 mph or 300–5000 rpm are rejected, and each event number is saved only once.

The packet mapping is based on captures from the user's own ball and matching throw effort. Continue comparing several throws against the official app before treating it as production-calibrated. Movement, spin axis and release metrics remain unmapped.

The app is an original prototype, not PitchLogic's branded app. No Android SDK or Gradle executable was available in the creation environment; the project has not yet been compiled on a physical Android device.

## Virtual replay

Tap **Replay previous pitch** after saving a pitch. The 3D-styled field scene shows an animated ball from mound to plate with recorded pitch type, speed and spin. Use play/pause, restart, speed and the scrub bar. The path is explicitly illustrative; pitch type and spin do not determine its shape. When verified sensor values for movement, release and spin axis become available, the replay can use those measurements.

## Bullpen flow

The Bullpen screen opens directly to connection, manual pitch capture, recent pitches and export. Tap any recent pitch to replay that specific entry. The replay includes stadium lighting, a strike zone, motion trail, speed/spin overlay, scrub bar, pause, restart and playback speed. These graphics are original 2D Canvas rendering and do not represent verified 3D tracking.

## PitchLogic-style product direction

The intended release flow is: connect once, select pitch type, start a bullpen, throw normally, and receive an automatic pitch card plus replay. The source now has the live speed/spin foundation for that workflow. The next capture-validation gates are spin direction/axis, efficiency, movement, release orientation, and the raw-sample trajectory model; those fields should not be labeled until matched against known official readings.

## Release gate

For a turnkey ball-connected release, test with the actual ball, identify any required connection handshake and authorized data format, validate speed/spin/movement against known readings, handle disconnects, test on the target phone, and produce a signed APK. None of those sensor claims can be established from source alone.
