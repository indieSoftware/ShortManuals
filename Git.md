## Install git

Install git via brew:

	brew install git
	
Createn `~/.gitconfig`:

	touch ~/.gitconfig
	
Add content with the follwing example:

	[user]
	email = sven.korset@indie-software.de
	name = Sven Korset
	[core]
	excludesfile = /Users/sven/.gitignore_global
	[difftool "sourcetree"]
	cmd = opendiff \"$LOCAL\" \"$REMOTE\"
	path = 
	[mergetool "sourcetree"]
	cmd = /Applications/Sourcetree.app/Contents/Resources/opendiff-w.sh \"$LOCAL\" \"$REMOTE\" -ancestor \"$BASE\" -merge \"$MERGED\"
	trustExitCode = true
	[commit]
	template = /Users/sven/.stCommitMsg

Create global ignore list:

	~/.gitignore_global
	
Add:

	*~
	.DS_Store

Install Sourcetree as UI: [https://www.sourcetreeapp.com](https://www.sourcetreeapp.com)

## Add ssh keys for git

Create or copy your ssh & .pub files into

	~/.ssh
	
E.g.

	~/.ssh/SvenKorset-GitHub
	~/.ssh/SvenKorset-GitHub.pub

To create a new ssh key:

	ssh-keygen -f ~/.ssh/SvenKorset-GitLab -t rsa -b 4096 -C "An optional comment to add at the end of the public key"

Add each ssh key to the chain and add password:

	ssh-add -K ~/.ssh/SvenKorset-GitHub
	
Add a config:

	~/.ssh/config
	
With the content:

	Host SvenKorset-Bitbucket
	HostName bitbucket.org
	User SvenKorset
	PreferredAuthentications publickey
	IdentityFile /Users/sven/.ssh/SvenKorset-Bitbucket
	UseKeychain yes
	AddKeysToAgent yes

	Host indieSoftware-GitHub
	HostName github.com
	User indieSoftware
	PreferredAuthentications publickey
	IdentityFile /Users/sven/.ssh/indieSoftware-GitHub
	UseKeychain yes
	AddKeysToAgent yes

Check if they are loaded:

	ssh-add -l

Each ssh key should get listed.

To remove one particular key:

	ssh-add -d file

Or to remove all keys:

	ssh-add -D

To add all again:

	ssh-add -A

## Load ssh keys when the user logs in

Create a launchd daemon which starts a shell script, by creating a new file at 

	~/Library/LaunchAgents/com.user.loginscript.plist
	
Add the following content:

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
	<plist version="1.0">
	<dict>
		<key>Label</key>
		<string>com.user.loginscript.plist</string>
		<key>ProgramArguments</key>
		<array>
			<string>/bin/sh</string>
			<string>/Users/sven/onLogin.sh</string>
		</array>
		<key>RunAtLoad</key>
		<true/>
		<key>KeepAlive</key>
		<false/>
	</dict>
	</plist>

Create the shell script which will be called by the daemon:

	~/onLogin.sh
	
With the content:

	#!/bin/sh

	# load ssh keys for git & co
	ssh-add -A 2>/dev/null;

## Use Microsoft Git Credential Manager for Visual Studio Team Services

[https://github.com/Microsoft/Git-Credential-Manager-for-Mac-and-Linux](https://github.com/Microsoft/Git-Credential-Manager-for-Mac-and-Linux)

Install via Homebrew (modifies the global Git configuration file `.gitconfig` in the home folder for a `credential` section):

	brew install git-credential-manager
	git-credential-manager install

Now the git credential manager should be ready and will automatically be used when using git commands:

	git clone urlToRepo

When an error occured while using the git credential manager it might happen because the manager needs Java 1.8. For this to fix modify the `.gitconfig` in the home folder and change the path to the java virtual machine pointing to version 1.8 (needs to be installed):

	[credential]
	helper = !/Library/Java/JavaVirtualMachines/jdk1.8.0_162.jdk/Contents/Home/bin/java -Ddebug=false -Djava.net.useSystemProxies=true -jar /usr/local/Cellar/git-credential-manager/2.0.3/libexec/git-credential-manager-2.0.3.jar
