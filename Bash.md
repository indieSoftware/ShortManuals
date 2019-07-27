Bash
===

Read [https://itnext.io/upgrading-bash-on-macos-7138bd1066ba](https://itnext.io/upgrading-bash-on-macos-7138bd1066ba) why and how to install a new bash.

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
