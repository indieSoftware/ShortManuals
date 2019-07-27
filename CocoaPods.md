CocoaPods
===


## Links

Website: [https://cocoapods.org/](https://cocoapods.org/)

## Install

Best use RVM to install ruby versions and gemsets: [https://rvm.io](https://rvm.io)

Second choice, install cocapods via homebrew:

	brew install cocoapods

Check pod version:

	pod --version

## Pod file

Go to the Xcode project folder and create a new `Podfile`:

	touch Podfile & open Podfile

Add something like:

```
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '10.0'
use_frameworks!

# Required common pods.
def common_pods
	# Constants for localized text, storyboards, nibs, etc.
	# https://github.com/mac-cain13/R.swift
	pod 'R.swift'
	
	# Add more pods here...
end

# Pods used for development only.
def development_pods
	# A linter to gather code metrics.
	# https://github.com/realm/SwiftLint
	pod 'SwiftLint'

	# Formats the swift code to be consistent.
	# https://github.com/nicklockwood/SwiftFormat
	pod 'SwiftFormat/CLI'
end

# Pods for testing.
def test_pods
	# Mocking framework for Swift.
	# https://github.com/Brightify/Cuckoo
	pod 'Cuckoo'

	# HTTP request mocker.
	# https://github.com/kylef/Mockingjay/releases
	pod 'Mockingjay'
end


# Targets

target 'MyTargetName' do
	common_pods
	development_pods
end

target 'MyTargetNameUnitTests' do
	test_pods
end

target 'MyTargetNameUITests' do
	test_pods
end


# Post install

post_install do |installer|
	installer.pods_project.targets.each do |target|
		target.build_configurations.each do |config|
			# Ignore any warnings from pods.
			config.build_settings['GCC_WARN_INHIBIT_ALL_WARNINGS'] = "YES"
			config.build_settings['SWIFT_SUPPRESS_WARNINGS'] = "YES"
		end
	end
end
```

## Commands

Update the local pod repo:

	pod repo update
	
Install new pods added to the podfile or as initial set up:

	pod install

When new versions of pods are released update the local version with:

	pod update	

