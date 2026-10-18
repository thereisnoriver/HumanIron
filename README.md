# HumanIron — Camera-Based Posture and Blink Reminder Prototype

HumanIron is a **2020 two-person team project** exploring a Raspberry Pi camera system that provides reminders for desk posture, blinking, and stretching. This repository is a fork of the [original team repository](https://github.com/icymunchhhiip/HumanIron).

The team **completed and tested the prototype on physical hardware in 2020**. The code is preserved as a historical project and has **not** been revalidated on current Raspberry Pi hardware or with today's third-party APIs. The project's individual implementation responsibilities cannot be reliably reconstructed from the available Git history, so no individual feature ownership is claimed here.

## Prototype features

| Feature | Implementation in the repository |
| --- | --- |
| Posture reminder | Captures a reference image and later frames; sends images to an external pose-estimation endpoint; compares selected nose/shoulder keypoints and plays a correction sound when a fixed threshold is exceeded. |
| Blink reminder | Uses dlib's 68-point facial landmark model, a horizontal/vertical eye ratio, and an audio prompt if no blink is detected for a configured interval. |
| Stretch reminder | Plays an audio prompt periodically with a rest interval. |
| Raspberry Pi interaction | Uses PiCamera and GPIO pin 17 for camera capture and button/relay experiments. |

The team completed the original project and performed physical-device tests in 2020, but the repository does not include test logs, measured accuracy, or a reproducible validation procedure. The modules are exploratory scripts rather than a currently validated, unified application. In particular, `mergedThreading.py` attempts to coordinate the features with asyncio, but its concurrent scheduling and error handling have not been verified.

## Technology

- Python
- Raspberry Pi, PiCamera, and RPi.GPIO
- OpenCV and dlib for image processing and facial landmarks
- NumPy, Matplotlib, and pycocotools for pose visualization
- Requests for a legacy external pose API
- pygame for audio cues

## Code map

- `mergedThreading.py` — experimental integrated workflow (posture, blink, stretching)
- `method1.py` — standalone posture experiment
- `blinkEyes.py` — standalone blink experiment using a USB/video camera
- `requireStretching.py` — timed audio reminder
- `detector.py` — Raspberry Pi face/eye detection and GPIO experiment
- `Motion.py` — Raspberry Pi camera and GPIO-triggered capture experiment
- `haarcascade_*.xml` — OpenCV cascade model files
- `shape_predictor_68_face_landmarks.dat` — legacy dlib landmark model
- `*.wav` — audio reminder assets

## Historical setup (not a verified installation guide)

1. Prepare a compatible Raspberry Pi with a supported PiCamera, GPIO wiring, and a display/audio output.
2. Install compatible historical versions of Python packages used in the selected script. No lockfile or tested package/version matrix was preserved.
3. From the repository root, select a standalone experiment, for example `python blinkEyes.py` on a machine with a camera, dlib model, and audio support.
4. For posture scripts, `KAKAO_POSE_API_KEY` must be supplied through the environment, not stored in source code. The hard-coded legacy API endpoint may no longer be available, so these scripts should be treated as reference code rather than a working integration.

**Do not assume these instructions work on current systems.** Camera drivers, legacy dependencies, permissions, data files, and service access need independent verification.

## Limitations and security notes

- No automated tests, benchmark results, accuracy measurements, or reproducible installation are included.
- Hard-coded thresholds for pose displacement and blink detection were experimental; they are not calibrated across users, camera placement, or lighting.
- Pose estimation depends on an external service and transmits captured images; do not run it with sensitive images without reviewing privacy implications.
- The historical Git history contains a hard-coded API credential in an older posture script. The current file reads its key from the environment, but **removing it from the current source does not remove it from Git history**. The original key owner should revoke or rotate it.
- The pose code's index arithmetic and asynchronous execution have not been validated. A successful execution or posture-correction efficacy is not claimed.
- No medical or ergonomic benefit has been established by this prototype.

## Attribution

Completed and physically tested as a two-person team project in 2020. The original source was published under [icymunchhhiip/HumanIron](https://github.com/icymunchhhiip/HumanIron), and this fork is maintained for educational and portfolio documentation. The inherited commits are attributed to the original repository account; they cannot establish each teammate's individual implementation scope.

No license file was present in the original project. Reuse of third-party model and media assets should be checked against their respective terms before redistribution or further development.
