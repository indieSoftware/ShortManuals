RVM
===

RVM is a command-line tool which allows you to easily install, manage, and work with multiple ruby environments from interpreters to sets of gems.

Use this instead of sudo gems!

## Links

[https://rvm.io](https://rvm.io)

## Install

Install RVM:

	\curl -sSL https://get.rvm.io | bash -s stable
	
Quit and relaunch Terminal.

Check RVM version (1.29.7 or higher):

	rvm -v

Ruby version (2.3.7 or higher):

	ruby -v

Updating RVM:

	rvm get stable

List known ruby versions:

	rvm list known

Install a ruby version:

	rvm install 2.6

See which ruby versions are installed:

	rvm list rubies
	
Set default ruby:

	rvm alias create default ruby-2.6.3

Make sure the RVM Ruby is used, not the OSX one:

	which ruby

Should return something like `/Users/skorset/.rvm/rubies/ruby-2.6.3/bin/ruby` and not `/usr/bin/ruby`. If the latter is the case keep in mind that with MacOS Catalina the zsh is used rather than the bash, therefore, the `~/.zshrc` needs to be configured instead of the `.bash_profile`.

## Gems

List gemset:

	rvm gemset list

Change gemset, e.g. to global:

	rvm gemset use global

Use global cache for gems:

	rvm gemset globalcache enable
	
List installed gems:

	gem list

Install gems now for a single user (no `sudo` needed anymore):

	gem install jazzy

Update Gems:

	rvm gemset update

Cleanup:

	rvm cleanup all

