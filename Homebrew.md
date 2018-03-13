## brew

Brew website: [https://docs.brew.sh/FAQ.html](https://docs.brew.sh/FAQ.html)

Update formulae and Homebrew itself:

	brew update

Install something like cocoapods:

	brew install cocoapods

Upgrade everything:

	brew upgrade

Clean up outdated formulae:

	brew cleanup

List all installed packages with their version number:

	brew list --versions

List all installed top-level packages (that are not dependencies):

	brew leaves

## tap

The `tap` command adds additional repositories of formulae.

List all tapped repos:

	brew tap
	
Add a tap, e.g. for `cask` (not necessary anymore because it's part of brew):

	brew tap caskroom/cask

Remove a tap:

	brew untap caskroom/versions

## cask

Cask website: [https://gillesfabio.github.io/homebrew-cask-homepage/](https://gillesfabio.github.io/homebrew-cask-homepage/)

Install software via cask:

	brew cask install fastlane

Uninstall software:

	brew cask uninstall google-chrome

List all via cask installed software:

	brew cask list

List outdated casks:

	brew cask outdated

Update all outdated casks:

	brew cask upgrade

## versions

Add older version to brew:

	brew tap caskroom/versions

Find older versions of java:

	brew cask search java
	
Install older version of java:

	brew cask install java8

### jenv

Use `jenv` [http://www.jenv.be](http://www.jenv.be) to manage multiple java versions and their `JAVA_HOME` environment.

	brew install jenv

Add to `~/.bash_profile`:

	export PATH="$HOME/.jenv/bin:$PATH"
	eval "$(jenv init -)"

Register JDKs (look in the `/Library/Java/JavaVirtualMachines` folder to find the available versions):

	jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_162.jdk/Contents/Home/
	jenv add /Library/Java/JavaVirtualMachines/jdk-9.0.4.jdk/Contents/Home/
	
List all registered JDKs (the one with the asterisk is the active one):

	jenv versions

Result may look similar to:

	* system (set by /Users/sven/.jenv/version)
	1.8
	1.8.0.162
	9.0
	9.0.4
	oracle64-1.8.0.162
	oracle64-9.0.4

Change the global java version to 9:

	jenv global 9.0

Change the local java version which means all projects in this folder will use the provided version instead of the global one. Creates a `.java-version` file in the folder.

	jenv local 1.8

