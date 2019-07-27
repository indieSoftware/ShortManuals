# Some Xcode tips and tricks

## Build settings

To find out more about available placeholders to use in build scripts or sort of, e.g. `${SRCROOT}` open Xcodes help and search for `build settings`. Or open the website: [https://help.apple.com/xcode/mac/10.2/#/itcaec37c2a6](https://help.apple.com/xcode/mac/10.2/#/itcaec37c2a6)

To get the path behind such a variable, i.e. for `PROJECT_DIR` use the terminal and run:

	xcodebuild -project yourProject.xcodeproj -target yourTarget -showBuildSettings | grep PROJECT_DIR
	
Or when using a workspace:

	xcodebuild -workspace yourWorkspace.xcworkspace -scheme yourScheme -showBuildSettings | grep PROJECT_DIR

## Free up space

The folder `~/Library/Developer/CoreSimulator/Devices/` might get quite large after time. To list all devices:

	xcrun simctl list devices

And to clean up all unused devices:

	xcrun simctl delete unavailable

Accroding to [https://stackoverflow.com/questions/29930198/can-i-delete-data-from-ios-devicesupport](https://stackoverflow.com/questions/29930198/can-i-delete-data-from-ios-devicesupport) some folders can be deleted regularly to free up some space.

	~/Library/Developer/Xcode/iOS DeviceSupport
	~/Library/Developer/Xcode/DerivedData
	~/Library/Developer/Xcode/Archives
