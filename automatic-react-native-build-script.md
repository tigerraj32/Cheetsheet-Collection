# Build Scripts for iOS and Android React Native Projects

This document provides scripts for automating the build process for iOS and Android React Native projects. These scripts handle multiple build flavors and schemes, generating signed `.ipa` and APK files for release.

## Prerequisites

- **macOS** with Xcode installed (for iOS).
- **Android Studio** and the Android SDK installed (for Android).
- **Gradle** configured in your React Native project.
- **Keystore file** for signing APKs.

## iOS Build Script

This script builds and signs an `.ipa` file for multiple schemes in a React Native iOS project.

### Script

```bash
#!/bin/bash

# Set the path to your React Native project directory
PROJECT_PATH="/path/to/your/react-native-project"

# Set the scheme and configuration (Debug or Release)
SCHEMES=("Scheme1" "Scheme2")  # Replace with your actual scheme names
CONFIGURATION="Release"

# Set the output directory for the final .ipa file
IPA_OUTPUT_DIR="$PROJECT_PATH/mobile_builds"

# Set the path to the archive and temporary build directory
TEMP_BUILD_DIR="$PROJECT_PATH/output"

# Set the path to the export options plist file
EXPORT_OPTIONS_PLIST="$PROJECT_PATH/ios/ExportOptions.plist"

# Ensure the output directory exists
mkdir -p "$IPA_OUTPUT_DIR"

# Navigate to the iOS project directory
cd "$PROJECT_PATH/ios"

# Clean the build
xcodebuild clean -workspace "$PROJECT_PATH/ios/YourWorkspace.xcworkspace" \
-scheme "$SCHEMES" \
-configuration "$CONFIGURATION"

# Loop through each scheme and build the .ipa
for SCHEME in "${SCHEMES[@]}"; do
    # Archive the project
    xcodebuild archive -workspace "$PROJECT_PATH/ios/YourWorkspace.xcworkspace" \
    -scheme "$SCHEME" \
    -configuration "$CONFIGURATION" \
    -archivePath "$TEMP_BUILD_DIR/$SCHEME.xcarchive" \
    -derivedDataPath "$TEMP_BUILD_DIR"

    # Export the archive to an .ipa file
    xcodebuild -exportArchive \
    -archivePath "$TEMP_BUILD_DIR/$SCHEME.xcarchive" \
    -exportOptionsPlist "$EXPORT_OPTIONS_PLIST" \
    -exportPath "$TEMP_BUILD_DIR"

    # Find the .ipa file and move it to the final output directory
    find "$TEMP_BUILD_DIR" -name "*.ipa" -exec mv {} "$IPA_OUTPUT_DIR/" \;

    echo "IPA for $SCHEME created at $IPA_OUTPUT_DIR"
done

# Clean up temporary build files
rm -rf "$TEMP_BUILD_DIR"
```

### Usage
- Replace /path/to/your/react-native-project with your actual project path.
- Replace Scheme1 and Scheme2 with your actual scheme names.
- Run the script: chmod +x build-ipa.sh && ./build-ipa.sh.

# Android Build Script

This script builds and signs APK files for multiple flavors in a React Native Android project.

```bash
#!/bin/bash

# Set the path to your React Native project directory
PROJECT_PATH="/path/to/your/react-native-project"

# Set the output directory for the final APK files
APK_OUTPUT_DIR="$PROJECT_PATH/mobile_builds"

# Define the list of flavors
FLAVORS=("flavor1" "flavor2")  # Replace with your actual flavor names

# Ensure the output directory exists
mkdir -p "$APK_OUTPUT_DIR"

# Navigate to the Android project directory
cd "$PROJECT_PATH/android"

# Clean the Android project
./gradlew clean

# Loop through each flavor and build the APK
for FLAVOR in "${FLAVORS[@]}"; do
    # Assemble the signed release APK for the current flavor
    ./gradlew assemble${FLAVOR^}Release

    # Set the path to the generated APK for the current flavor
    APK_PATH="$PROJECT_PATH/android/app/build/outputs/apk/$FLAVOR/release/app-$FLAVOR-release.apk"

    # Move the generated APK to the final output directory
    mv "$APK_PATH" "$APK_OUTPUT_DIR/"

    echo "Signed APK for $FLAVOR created at $APK_OUTPUT_DIR"
done

# Clean up temporary build files (optional)
./gradlew clean

```

### Usage
- Replace /path/to/your/react-native-project with your actual project path.
- Replace flavor1 and flavor2 with your actual flavor names.
- Run the script: chmod +x build-apk.sh && ./build-apk.sh.

### Conclusion

These scripts help automate the process of building and signing .ipa and APK files for React Native projects with multiple flavors and schemes. 
Ensure that your build.gradle and ExportOptions.plist are correctly configured for signing.

