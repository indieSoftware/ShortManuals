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

Add to `~/.bash_profile`:

	export PATH="$HOME/.fastlane/bin:$PATH"

And make also sure the locale is set:

	export LC_ALL=en_US.UTF-8
	export LANG=en_US.UTF-8

Check fastlane version:

	fastlane -v

Check fastlane environment:

	fastlane env

Update fastlane:

	fastlane update_fastlane

Clean gems:

	sudo gem cleanup

Install bundler ([http://bundler.io](http://bundler.io)):

	gem install bundler

Create a `Gemfile` to the project folder and add the dependencies for fastlane:

	source 'https://rubygems.org'
	gem 'xcode-install'
	gem 'xcodeproj'
	gem 'fastlane'

Install gems and dependencies (will be called automatically when executing fastlane via bundle):

	bundle install

List any outdated gems:

	bundle outdated

List all installed gems:

	bundle list

Update gems:

	bundle update

Install additional plugins (e.g. "appicon" [https://github.com/KrauseFx/fastlane-plugin-appicon](https://github.com/KrauseFx/fastlane-plugin-appicon)):

	fastlane add_plugin appicon

## Usage

Go to the project folder with the Gemfile and run the following to initiate fastlane:

	bundle exec fastlane init

After that fastlane is set up and can be used. Look at the fastlane folder to edit the config files and to tweak the lanes.

For getting the exact version number of Xcode run:

	xcversion list

To show all lanes and select one to run:

	bundle exec fastlane

Or run a specific lane directly (e.g. 'test'):

	bundle exec fastlane test

Modify the `fastfile` and see [https://docs.fastlane.tools](https://docs.fastlane.tools) for more information.

## Appfile

Modify the `Appfile` in the project's `fastlane` folder.

	app_identifier "de.indie-software.MyApp"
	apple_id "indie-software"
	team_name "Sven Korset"
	team_id "H6T8HJZGHY"

Make sure the app is with the app identifier exists in the Apple Developer Portal and of course also the other values match.

Also make sure the Mac has the private key for signing in its keychain. Provide the password via the [credentials_manager](https://github.com/fastlane/fastlane/tree/master/credentials_manager):

	fastlane fastlane-credentials add --username indie-software

To remove the password call:

	fastlane fastlane-credentials remove --username indie-software

## Provision quick info

Not necessary, but might be helpful: [https://github.com/ealeksandrov/ProvisionQL](https://github.com/ealeksandrov/ProvisionQL)

	brew cask install provisionql
