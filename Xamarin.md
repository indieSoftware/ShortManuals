# Installation

## Xamarin

Download `Xamarin` from [https://www.xamarin.com](https://www.xamarin.com).

## Visual Studio

As the IDE you should use `Visual Studio for Mac`: [https://www.visualstudio.com/de/vs/visual-studio-mac](https://www.visualstudio.com/de/vs/visual-studio-mac)

## Xcode

For iOS builds Apple's Xcode is needed to be installed on a Mac: [https://itunes.apple.com/de/app/xcode/id497799835?mt=12](https://itunes.apple.com/de/app/xcode/id497799835?mt=12)

## Device Manager

On Mac you have to install the Xamarin Device Manager manually: [https://developer.xamarin.com/guides/android/getting_started/installation/android-emulator/xamarin-device-manager/](https://developer.xamarin.com/guides/android/getting_started/installation/android-emulator/xamarin-device-manager/)

## Intel HAXM

Intel HAXM accelerates the Android emulator on Intel CPUs which modern MacBooks use.

Installing the HAXM driver with Xamarin's `SDK Manager` via `Android` → `Tools` → `Extra` → `Intel x86 Emulator Accelerator (HAXM installer)` doesn't seem to work. Also using `brew cask` doesn't seem to work and is also pointing to an old version (prio v7). 

So in this case install it directly and manually from Intel's website: [https://software.intel.com/en-us/articles/intel-hardware-accelerated-execution-manager-intel-haxm](https://software.intel.com/en-us/articles/intel-hardware-accelerated-execution-manager-intel-haxm)

Run the installer from the `dmg` file in the downloaded package.

To check if HAXM is installed and which version just run:

	kextstat | grep intel

This should show something like the following:

	176    0 0xffffff7f891e3000 0x1e000    0x1e000    com.intel.kext.intelhaxm (7.0.0) 823315F2-1275-377E-AFF9-E909857C3B2B <7 5 4 3 1>

In this case version 7.0.0 is installed and running.

# Setup

Start `Visual Studio`.

You might want to sign up for an Account to use Visual Studio Community edition.

## Update

First check for updates via `Visual Studio Community` → `Check for Updates`.

## Preferences

Open `Visual Studio Community` → `Preferences` → `Projects` → `SDK Locations`.

For `Apple` make sure the SDK's location is correctly provided.

For `Android` go through the tabs:

### Platforms

Install some platforms by checkmarking at least the sub-entries `Android SDK Platform` and a `System Image` of that target to test the app on the emulator.

### Tools

Make sure the most recent version of `Android SDK Tools` is used. So use version 26.1.1 (or higher) rather than the old version 25.2.5.

The `Android SDK Platform-Tools` should also be up to date.

Install the most recent version of the `Android SDK Build Tools`, e.g. 27.0.3.

Under `Extras` the `Intel x86 Emulator Accelerator (HAXM installer)` is not needed when it has been installed manually as described above.

You can consider to optionally install the Android SDK documentation via `Other` → `Documentation for Android SDK`.

### Locations

Make sure all locations are set correctly. The `Android SDK Location` and `Android NDK Location` should already be set correctly by the Xamarin installer.

However, the JDK might be installed manually to get a specific version. If this is the case provide here the correct path to the desired Java version.

# Project

## Quick start

You might want to set up a Hello World app as quick start: [https://developer.xamarin.com/guides/xamarin-forms/getting-started/hello-xamarin-forms/quickstart/](https://developer.xamarin.com/guides/xamarin-forms/getting-started/hello-xamarin-forms/quickstart/)

Choose `New Project` → `Multiplatform` → `Blank Forms App`.

Fill out the forms and make sure `Android` and `iOS` are checked. `Use Portable Class Library` should be selected and `Use XAML for user interface files` also checked.

Now the app should already be runnable. Try to run the `.Droid` and the `.iOS` projects with any device / emulator / simulator.

## OS Version

### iOS

`Info.plist`:

* `Deployment Target` defines the minimal iOS version on which the app can run.
* `Device family` can restrict the devices to iPhone or iPad only or to support both with `Universal`.

Project's `Options`:

* `iOS Build` → `SDK version` defines the SDK to use and with being set to `Default` should always be the latest.

To run the app on simulators with specific iOS versions they have first to be installed via Xcode. When the simulators have been installed in Xcode the specific iOS version to use for the simulator can be chosen in the device drop-down menu in Visual Studio.

### Android

Project `Options` → `General`:

* `Target framework` should be `Use latest installed platform` to use the latest installed SDK for building the app.

`AndroidManifest.xml` or project `Options` → `Android Application`:

* `Minimum Android version` defines the minimum API level supported.
* `Target Android version` should be set to `Automatic` to use always the latest SDK / API level available on the device.

With the new Android SDK Tooks v26 and higher it's currently not possible to install emulator images targeting Android API levels prior to 24 (Android 7.0 Nougar).

So when trying to debug on an emulator with an older Android version use the old `Andrid SDK Tools 25` and `Google Emulator Manager` to set up the emulator. But when doing this and switching later back to the new Tools the emulator might not work anymore, always terminating with an error when trying to start. To repair this re-run the `Visual Studio for Mac Installer` and install the `Android SDK` again.

## App Version

### iOS

`Info.plist`:

* `Version` is the logical version number, e.g. `1.0`.
* `Build` is the app's build number, e.g. `42`.

### Android

`AndroidManifest.xml`:

* `Version name` is the logical version number, e.g. `1.0`.
*  `Version name` is the app's build number, e.g. `42`.

