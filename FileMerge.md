FileMerge needs opendiff to work and point to the Xcode command line tools.

Call in terminal:

	opendiff
	
If it fails:

	sudo xcode-select -s /Applications/Xcode.app

After that Sourcetree should be able to open FileMerge for resolving conflicts and diffs.
