# Android SDK Builder for Godot (Linux)

This repository uses **GitHub Actions** to build a clean Android SDK for **Godot Engine**.

It is intended for developers who cannot reliably download Android SDK packages directly from Google due to slow or blocked connections.

The workflow runs on GitHub's Ubuntu runners, downloads only the required SDK packages, and produces a portable SDK archive that can be downloaded and used locally.

---

## Features

- Downloads only the packages required by Godot.
- Produces a clean SDK (no emulator or extra packages).
- Runs entirely on GitHub.
- Works even if your local internet connection cannot download from Google.
- Easy to update for future Godot versions.

---

## Current Godot 4.7 SDK Packages

The workflow currently installs:

- platform-tools
- build-tools 35.0.1
- Android Platform 35
- Command Line Tools (latest)
- CMake 3.10.2.4988404
- NDK 28.1.13356709

These correspond to the current Godot documentation.

---

## Repository Structure

```
.github/
└── workflows/
    └── build-sdk.yml
```

---

## Running the Workflow

1. Open the repository.
2. Select **Actions**.
3. Select **Build Android SDK for Godot**.
4. Click **Run workflow**.
5. Wait for the workflow to finish.
6. Download the generated artifact named:

```
godot-sdk
```

---

## Extracting the SDK

GitHub downloads the artifact as a ZIP file.

Extract it.

Inside you will find

```
godot-sdk.tar
```

Extract it again.

The final folder should look like

```
godot-sdk
├── build-tools
├── cmdline-tools
├── cmake
├── licenses
├── ndk
├── platform-tools
└── platforms
```

---

## Using with Godot

Open

Editor

→ Editor Settings

→ Export

→ Android

Set

Android SDK Path

to

```
/path/to/godot-sdk
```

Also install OpenJDK 17 and set

Java SDK Path

to your local JDK installation.

Example (Linux):

```
/usr/lib/jvm/java-17-openjdk-amd64
```

---

## Updating for Future Godot Versions

When a new version of Godot changes the Android SDK requirements, only edit the package list inside

```
.github/workflows/build-sdk.yml
```

Locate:

```yaml
sdkmanager --sdk_root="$GODOT_SDK" \
  "platform-tools" \
  "build-tools;35.0.1" \
  "platforms;android-35" \
  "cmdline-tools;latest" \
  "cmake;3.10.2.4988404" \
  "ndk;28.1.13356709"
```

Change only the versions required by Godot.

Example:

```yaml
sdkmanager --sdk_root="$GODOT_SDK" \
  "platform-tools" \
  "build-tools;36.0.0" \
  "platforms;android-36" \
  "cmdline-tools;latest" \
  "cmake;3.31.5" \
  "ndk;29.0.xxxxxxxx"
```

Commit the change and run the workflow again.

A new SDK artifact will be generated.

---

## Finding Required Versions

Always check the official Godot Android export documentation for the required versions.

The documentation usually contains a command similar to

```
sdkmanager \
"platform-tools" \
"build-tools;35.0.1" \
"platforms;android-35" \
"cmdline-tools;latest" \
"cmake;3.10.2.4988404" \
"ndk;28.1.13356709"
```

Copy those package names directly into the workflow.

---

## Notes

This repository only builds the Android SDK.

It does not install:

- OpenJDK
- Godot Export Templates

These must be installed locally.

---

## License

MIT
