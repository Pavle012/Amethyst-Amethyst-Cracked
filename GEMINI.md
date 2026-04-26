# Amethyst Cracked - Project Instructions

## Project Overview
Amethyst Cracked is a fork of the Amethyst launcher, which is itself based on PojavLauncher. It is a Minecraft: Java Edition launcher for Android that allows playing the game without a paid Microsoft account (Cracked/Offline support).

### Key Technologies
- **Language:** Java (Android), C (JNI)
- **Build System:** Gradle
- **Graphics:** OpenGL ES (via GL4ES/Zink), LWJGL 3
- **Runtime:** OpenJDK 8/17/21 (Custom builds for Android)
- **Architecture:** Multi-module Android project with native hooks for JVM launching and input management.

### Architecture & Modules
- `:app_pojavlauncher`: The main Android application module.
- `:jre_lwjgl3glfw`: Stub for LWJGL 3 GLFW support.
- `:arc_dns_injector`: DNS injection utility.
- `:methods_injector_agent`: Java agent for bytecode manipulation and mod compatibility.
- `:forge_installer`: Helper for installing Minecraft Forge.

## Building and Running
The project uses Gradle for building. JREs are typically downloaded from CI during the build process.

### Build Commands
- **Build Debug APK:** `./gradlew :app_pojavlauncher:assembleDebug`
- **Build Release APK:** `./gradlew :app_pojavlauncher:assembleRelease`
- **Clean Project:** `./gradlew clean`
- **Build GLFW Stub:** `./gradlew :jre_lwjgl3glfw:build`

### Setup Requirements
- Run the language list updater before building:
  ```bash
  chmod +x scripts/languagelist_updater.sh
  ./scripts/languagelist_updater.sh
  ```

## Development Conventions
- **Offline/Cracked Support:** The project aims to keep features unlocked for local (offline) accounts. Always check `Tools.hasOnlineProfile()` or related logic when adding features to ensure they remain accessible to non-premium users.
- **Branding:** Use "Amethyst Cracked" for user-facing strings and logs.
- **Native Logic:** Core JVM launching logic is found in `app_pojavlauncher/src/main/jni` and called via `JREUtils`.
- **Package Name:** The application ID is `org.pavle012.amethystcracked`.

## Key Files
- `app_pojavlauncher/src/main/java/net/kdt/pojavlaunch/Tools.java`: Central utility class containing constants, directory paths, and core logic.
- `app_pojavlauncher/src/main/java/net/kdt/pojavlaunch/value/MinecraftAccount.java`: Defines the account structure (Online vs. Local).
- `app_pojavlauncher/build.gradle`: Main build configuration and app metadata.
- `app_pojavlauncher/src/main/jni/jre_launcher.c`: Native entry point for starting the JVM.
