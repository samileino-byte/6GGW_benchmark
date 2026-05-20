Based on DRAMM, CPU, GNU performance upgrade for 6G network in 5G.


Benchmark stability improvements done:

1. Timeout protection - each benchmark type has a timeout (CPU/Memory 20s, GPU/DRAM 30s, Continuous 60s). If exceeded, shows [TIMEOUT] instead of hanging.

2. Memory safety - automatically detects available RAM and adjusts buffer sizes:
- Low RAM (under 256MB free): uses 2MB buffers, 8MB DRAM probe
- Medium RAM (under 512MB free): uses 4MB buffers, 16MB DRAM probe
- Normal: uses default 10MB/32MB

3. OOM protection - catches OutOfMemoryError, shows [OOM] with available RAM, triggers garbage collection

4. Error recovery - all benchmarks wrapped in try/catch. If a benchmark crashes (native code, GPU driver issue), shows error message and recovers to ready state instead of freezing the app.

5. Frame timing error handling - catches exceptions from Choreographer/GPU setup

6. Version bumped to 1.1.0, shows available RAM at startup


BOOTLOADER
v1.1.0 now includes full Android and iOS distributions:

ANDROID (android/ folder):
- jni/Android.mk + Application.mk - NDK ndk-build (arm64-v8a, armeabi-v7a, x86_64, x86)
- jni/ai2orbit_boot_jni.cpp - JNI bridge with phone and tablet init
- AI2ORBITBoot.kt - Kotlin wrapper class, drop into your app
- CMakeLists.txt - Alternative CMake NDK build
- Phone: AI2ORBITBoot().init() then .run()
- Tablet: AI2ORBITBoot().initTablet() then .run()
- Returns String array of boot screen text lines for rendering in TextView

iOS (ios/ folder):
- ai2orbit_boot_c.h/.cpp - Pure C bridge (Swift cant call C++ directly)
- AI2ORBITBoot.swift - Swift wrapper class
- CMakeLists.txt - Xcode build (cmake -G Xcode -DCMAKE_SYSTEM_NAME=iOS)
- iPhone: AI2ORBITBoot().run()
- iPad: AI2ORBITBoot().run(isTablet: true)
- Auto-detects iPad via UIDevice.current.userInterfaceIdiom == .pad

Form factor tuning:
- Phone: 1MB DRAM blocks, 24-char loader, 40K init iterations
- Tablet: 2MB DRAM blocks, 36-char loader, 60K init iterations
- Desktop: 4MB DRAM blocks, 50-char loader, 100K init iterations

Also included: lib/ (core library), xda/ (separate XDA distribution), desktop CLI, Makefile, CMakeLists.txt for Linux/Windows/macOS.
9:
