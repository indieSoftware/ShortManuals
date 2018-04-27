# General

## Hidden files

In Finder the hidden files aren't shown per default. To make them visible temporarily do the following:

1. Open Finder
2. Go to the `Macintos HD` root folder (access from Devices in the left).
3. Hold down `cmd` + `shift` + `.` (dot)

Now all hidden files should be visble, look for the otherwise hidden `Library` folder.

To hide the file again, press again `cmd` + `shift` + `.` on the root folder in Finder.

Alternatively to make hidden files visible permanently use the console by executing:

	defaults write com.apple.finder AppleShowAllFiles YES

Then relaunch Finder by holding the `alt` key, then pressing with the right mouse button on the Finder icon in the dock and select `Relaunch`.

## Locale

When using cocoapods or other script and an error occures like:

	perl: warning: Setting locale failed.
	perl: warning: Please check that your locale settings:
		LANGUAGE = (unset),
		LC_ALL = (unset),
		LC_CTYPE = "UTF-8",
		LANG = "en_US"
	    are supported and installed on your system.

First check your currently set locale in console:

	locale

The error may occur when the `locale` output doesn't look like:

	LANG=
	LC_COLLATE="en_US.UTF-8"
	LC_CTYPE="en_US.UTF-8"
	LC_MESSAGES="en_US.UTF-8"
	LC_MONETARY="en_US.UTF-8"
	LC_NUMERIC="en_US.UTF-8"
	LC_TIME="en_US.UTF-8"
	LC_ALL="en_US.UTF-8"

Make sure en_US and UTF-8 locales are installed by listing all installed locales:

	locale -a

Then add the following lines to `~/.bash_profile`:

	export LC_CTYPE=en_US.UTF-8
	export LC_ALL=en_US.UTF-8

Create the file if it doesn't exist with:

	touch .bash_profile

Close the terminal and re-open it again to have the locales applied.
