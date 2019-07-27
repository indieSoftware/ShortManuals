Carthage
===


## Links

Website: [https://github.com/Carthage/Carthage](https://github.com/Carthage/Carthage)

## Install

Install Carthage via homebrew:

	brew install carthage

If an error occured like

	Error: An unexpected error occurred during the `brew link` step
	The formula built, but is not symlinked into /usr/local
	Permission denied @ dir_s_mkdir - /usr/local/Frameworks
	Error: Permission denied @ dir_s_mkdir - /usr/local/Frameworks

Then run the following:

	sudo mkdir /usr/local/Frameworks
	sudo chown $(whoami):admin /usr/local/Frameworks

After that just link the version which failed before:

	brew link carthage

## Cartfile

Go to the Xcode project folder and create a new `Cartfile`:

	touch Cartfile & open Cartfile

Add something like:

```
github "Alamofire/Alamofire" ~> 4.7.2
```

## Commands

Update dependencies:

	carthage update
	
Or only for an iOS project:

	carthage update --platform iOS

Drag the built `.framework` binaries from `Carthage/Build/<platform>` into the application’s Xcode project.

On the application targets’ `Build Phases` settings tab, click the `+` icon and choose `New Run Script Phase`. Create a `Run Script` in which to specify the shell (ex: `/bin/sh`), add the following contents to the script area below the shell:

	/usr/local/bin/carthage copy-frameworks
	
Add the paths to the frameworks to use under “Input Files".

	$(SRCROOT)/Carthage/Build/iOS/Alamofire.framework

Add the paths to the copied frameworks to the “Output Files”.

	$(BUILT_PRODUCTS_DIR)/$(FRAMEWORKS_FOLDER_PATH)/Alamofire.framework

## Additional commands

Build all the dependencies without fetching them.

	carthage build
	
Show the outdated dependencies.

	carthage outdate
