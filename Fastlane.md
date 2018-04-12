Fastlane
===


## Links

Fastlane website: [https://fastlane.tools/](https://fastlane.tools/)

Installing fastlane: [https://docs.fastlane.tools/getting-started/ios/setup/](https://docs.fastlane.tools/getting-started/ios/setup/)

Fastlane actions doc: [https://docs.fastlane.tools/actions/](https://docs.fastlane.tools/actions/)


## Xcode

Install Xcode command line tools:

	xcode-select --install

Check the current Xcode version:

	xcodebuild -version

## Install

Install fastlane via homebrew:

	brew cask install fastlane

Check fastlane version:

	fastlane -v

Check fastlane environment:

	fastlane env

Update fastlane:

	fastlane update_fastlane

Clean gems:

	sudo gem cleanup

Install bundler:

	gem install bundler

Add dependencies in a Gemfile in the project's root:

	source 'https://rubygems.org'
	gem 'xcode-install'
	gem 'xcodeproj'
	gem 'fastlane'

Install gems and dependencies:

	bundle install

List all installed gems:

	gem list

Update gems:

	bundle update

Install additional plugins (e.g. "appicon" [https://github.com/KrauseFx/fastlane-plugin-appicon](https://github.com/KrauseFx/fastlane-plugin-appicon)):

	fastlane add_plugin appicon

## Usage

Go to the project folder with the Gemfile and run the following to initiate fastlane:

	bundle exec fastlane init

After that fastlane is set up and can be used. Look at the fastlane folder to edit the config files and to tweak the lanes.

To show all lanes and select one to run:

	bundle exec fastlane

Or run a specific lane directly (e.g. 'test'):

	bundle exec fastlane test

