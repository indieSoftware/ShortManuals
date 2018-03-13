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

Close the terminal and re-open it again to have the locales applied.
