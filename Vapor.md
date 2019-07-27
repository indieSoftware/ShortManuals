Server-side Swift with Vapor
===

##Install##

1. Install Xcode
2. Install brew

Install Vapor via brew:

	brew tap vapor/homebrew-tap
	brew update
	brew install vapor
	
Confirm:

	vapor --help
	
##Create new project##

	vapor new projectName --template=twostraws/vapor-clean

- `projectName`: The name of the new project to create.
- `twostraws`: The git user which provides the template.
- `vapor-clean`: The template to use, in this case a minimal working one.

Build:

	vapor build
	
Run (terminate via Ctrl+C):

	vapor run

Update after adding packages (also creates a Xcode project):

	vapor update
	
Creating a Xcode project manually:

	vapor xcode

Running via Xcode:

- Select the `Run` target.
- Choose `My Mac` as device.
- ⌘R

