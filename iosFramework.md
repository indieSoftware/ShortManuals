## Creating an iOS Framework

### Framework's Build Settings

Add to `Runpath Search Paths`: `$(FRAMEWORK_SEARCH_PATHS)`

Otherwise it won't be possible to use `IBDesignable` because of the message `Library not loaded...` during rendering in Interface Builder.

### Create Build Pipeline

`File` → `New` → `Target...` → `Cross-platform` → `Aggregate`

Product Name: `BuildFramework`

Select `BuildFramework` Target → `Build Phases` → + → `New run script phase`

Content for the shell script:

```
#!/bin/sh
# Builds the framework as a device-only version for submitting to the App Store and as a universal framework for development on the simulator and a device.
# Inspired by https://stackoverflow.com/questions/24039470/xcode-6-ios-creating-a-cocoa-touch-framework-architectures-issues/26691080#26691080

# Name all dependencies here so they get bundled, too.
# Extend the array with spaces like 'dependencies=("Proj1" "Proj2" "Proj3")'
declare -a DEPENDENCIES=("WebViewJavascriptBridge")

# Define the final destination folder.
FINAL_DESTINATIONFOLDER=${PROJECT_DIR}/Products

# Define a temporary output folder.
UNIVERSAL_OUTPUTFOLDER=${BUILD_DIR}/${CONFIGURATION}-universal

# Set bash script to exit immediately if any commands fail.
echo "UB: Universal build started"
set -e

# Make sure all directories exists.
echo "UB: Create folders"
mkdir -p "${UNIVERSAL_OUTPUTFOLDER}"
mkdir -p "${FINAL_DESTINATIONFOLDER}/${CONFIGURATION}-iphoneos"

# Build device version via scheme
echo "UB: Build device binaries"
xcodebuild -workspace "${PROJECT_DIR}/${PROJECT_NAME}.xcworkspace" -scheme "${PROJECT_NAME}" ONLY_ACTIVE_ARCH=NO -configuration ${CONFIGURATION} -sdk iphoneos  BUILD_DIR="${BUILD_DIR}" BUILD_ROOT="${BUILD_ROOT}" clean build

# Build simulator version via scheme
echo "UB: Build simulator binaries"
xcodebuild -workspace "${PROJECT_DIR}/${PROJECT_NAME}.xcworkspace" -scheme "${PROJECT_NAME}" -configuration ${CONFIGURATION} -sdk iphonesimulator ONLY_ACTIVE_ARCH=NO BUILD_DIR="${BUILD_DIR}" BUILD_ROOT="${BUILD_ROOT}" clean build

# Copy the framework structure (from iphoneos build) to the universal folder.
echo "UB: Prepare universal bundle"
cp -R "${BUILD_DIR}/${CONFIGURATION}-iphoneos/${PROJECT_NAME}.framework" "${UNIVERSAL_OUTPUTFOLDER}/"
# And a copy to the final destination folder.
cp -R "${BUILD_DIR}/${CONFIGURATION}-iphoneos/${PROJECT_NAME}.framework" "${FINAL_DESTINATIONFOLDER}/${CONFIGURATION}-iphoneos/"

# Don't forget to copy all dependencies, too.
for DEPENDENCY_PROJECT in "${DEPENDENCIES[@]}"
do
  echo "UB: Copy '${DEPENDENCY_PROJECT}'"
  cp -R "${BUILD_DIR}/${CONFIGURATION}-iphoneos/${DEPENDENCY_PROJECT}/${DEPENDENCY_PROJECT}.framework" "${UNIVERSAL_OUTPUTFOLDER}/"
  cp -R "${BUILD_DIR}/${CONFIGURATION}-iphoneos/${DEPENDENCY_PROJECT}/${DEPENDENCY_PROJECT}.framework" "${FINAL_DESTINATIONFOLDER}/${CONFIGURATION}-iphoneos/"
done

# Copy the simulator binaries (if it exists) to the copied framework directory.
SIMULATOR_SWIFT_MODULES_DIR="${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${PROJECT_NAME}.framework/Modules/${PROJECT_NAME}.swiftmodule/."
if [ -d "${SIMULATOR_SWIFT_MODULES_DIR}" ]; then
  echo "UB: Copy simulator binaries"
  cp -R "${SIMULATOR_SWIFT_MODULES_DIR}" "${UNIVERSAL_OUTPUTFOLDER}/${PROJECT_NAME}.framework/Modules/${PROJECT_NAME}.swiftmodule"
fi

# Create universal binary file using lipo and place the combined executable in the output directory.
echo "UB: Create universal binary"
lipo -create -output "${UNIVERSAL_OUTPUTFOLDER}/${PROJECT_NAME}.framework/${PROJECT_NAME}" "${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${PROJECT_NAME}.framework/${PROJECT_NAME}" "${BUILD_DIR}/${CONFIGURATION}-iphoneos/${PROJECT_NAME}.framework/${PROJECT_NAME}"

# Create fat binary for each dependency framework
for DEPENDENCY_PROJECT in "${DEPENDENCIES[@]}"
do
  echo "UB: Lipo '${DEPENDENCY_PROJECT}'"
  lipo -create -output "${UNIVERSAL_OUTPUTFOLDER}/${DEPENDENCY_PROJECT}.framework/${DEPENDENCY_PROJECT}" "${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${DEPENDENCY_PROJECT}/${DEPENDENCY_PROJECT}.framework/${DEPENDENCY_PROJECT}" "${BUILD_DIR}/${CONFIGURATION}-iphoneos/${DEPENDENCY_PROJECT}/${DEPENDENCY_PROJECT}.framework/${DEPENDENCY_PROJECT}"
done

# Copy the framework to the final destination folder.
echo "UB: Copy universal framework"
cp -R "${UNIVERSAL_OUTPUTFOLDER}" "${FINAL_DESTINATIONFOLDER}/"

# Open the destination folder in Finder.
open "${FINAL_DESTINATIONFOLDER}"
echo "UB: Universal build finished"
```

Edit `BuildFramework` scheme → `Run` → `Info` → `Build Configuration` → `Release`

Build framework by running the `BuildFramework` scheme.
