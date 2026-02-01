# Building m8c-android

This guide explains how to build the `m8c-android` APK from source.

## Prerequisites

*   **Operating System:** Linux (Ubuntu 22.04+ recommended), macOS, or Windows (via WSL2).
*   **Java Development Kit (JDK):** JDK 11 or newer (JDK 21 is currently used in the environment).
*   **Android SDK:**
    *   Command Line Tools
    *   Platform Tools
    *   Platforms: `android-34`
    *   Build-Tools: `34.0.0`
*   **Android NDK:** Version `27.0.12077973` (installed automatically by Gradle).
*   **Git:** To clone the repository and submodules.

## Setup

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/v3rm0n/m8c-android.git
    cd m8c-android
    ```

2.  **Initialize Git Submodules:**
    This downloads the required dependencies (libusb, m8c, SDL2).
    ```bash
    git submodule update --init --recursive
    ```

    *Important Note:* This build relies on `SDL2`. The `m8c` submodule has recently migrated to SDL3. If you encounter build errors related to `SDL3`, you must manually checkout an older, SDL2-compatible version of `m8c` (e.g., `v1.7.2` or commit `59a4b9d`) and ensure the `SDL` submodule is on the `SDL2` branch.

    To force the correct versions:
    ```bash
    cd app/jni/SDL
    git checkout SDL2
    cd ../m8c
    git checkout v1.7.2
    cd ../../../
    ```

3.  **Configure Local Properties (Optional):**
    Create a `local.properties` file in the root directory if your Android SDK is in a custom location:
    ```properties
    sdk.dir=/path/to/your/android-sdk
    ```

## Building

1.  **Build the Debug APK:**
    Run the Gradle wrapper script:
    ```bash
    ./gradlew assembleDebug
    ```

2.  **Locate the APK:**
    Upon a successful build, the APK will be located at:
    `app/build/outputs/apk/debug/app-debug.apk`

## Troubleshooting

*   **SDL3 vs SDL2 Errors:** If you see errors like `make: *** No rule to make target 'SDL3'. Stop.`, it means the `Android.mk` configuration or submodules are out of sync. Ensure you have cleaned the build (`./gradlew clean`) and are using the correct submodule commits as described in the Setup section.
*   **Missing SDK:** Ensure `sdkmanager` is in your PATH or configured in `local.properties`.
