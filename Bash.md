Bash
===

Read [https://itnext.io/upgrading-bash-on-macos-7138bd1066ba](https://itnext.io/upgrading-bash-on-macos-7138bd1066ba) why and how to install a new bash.

**Beginning with MacOS 10.15 Catalina the OS comes with the default shell `zsh` instead of the old `bash`.**

Install via Homebrew:

	brew bash install

Edit shells file:

	sudo nano /etc/shells

Add to the bottom of file:

	/usr/local/bin/bash

Save and make the new shell the default one for terminal:

	chsh -s /usr/local/bin/bash

Re-open a new terminal and validate new version:

	echo $BASH_VERSION

## iTerm 2

An open source replacement for macOS' Terminal is [iTerm 2](https://www.iterm2.com).

To install it:

	brew cask install iterm2

