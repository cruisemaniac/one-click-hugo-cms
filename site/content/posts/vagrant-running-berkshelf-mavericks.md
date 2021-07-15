---
comments: true
date: "2014-09-09T19:00:00Z"
description: Get Vagrant running with berkshelf on Mavericks - A problem I faced and
  what was required to get this running.
tags:
- tech
- mac
- homebrew
- ops
- chef
- vagrant
- troubleshooting
title: Get Vagrant running with berkshelf on Mavericks
---

So, here I was trying to setup Vagrant on my Mac. Everything went fine till the ```vagrant up``` command and boom:

	myfirstvagrantproject $ vagrant up
	Bringing machine 'projectname' up with 'virtualbox' provider...
	==> projectname: The cookbook path '/Users/cruisemaniac/.berkshelf/projectname/vagrant/berkshelf-20140909-37664-1gf5t34-projectname' doesn't exist. Ignoring...
	Updating Vagrant's berkshelf: '/Users/cruisemaniac/.berkshelf/projectname/vagrant/berkshelf-20140909-37664-1gf5t34-projectname'
	RuntimeError: Couldn't determine Berks version: #<Buff::ShellOut::Response:0x00000101040360 @exitstatus=1, @stdout="", @stderr="/Users/cruisemaniac/.rbenv/versions/2.1.2/lib/ruby/2.1.0/rubygems/dependency.rb:298:in `to_specs': Could not find 'berkshelf' (>= 0) among 56 total gem(s) (Gem::LoadError)\n\tfrom /Users/cruisemaniac/.rbenv/versions/2.1.2/lib/ruby/2.1.0/rubygems/dependency.rb:309:in `to_spec'\n\tfrom /Users/cruisemaniac/.rbenv/versions/2.1.2/lib/ruby/2.1.0/rubygems/core_ext/kernel_gem.rb:53:in `gem'\n\tfrom /Users/cruisemaniac/.rbenv/versions/2.1.2/bin/berks:22:in `<main>'\n">

I knew vagrant liked ```chef_solo``` and ```berkshelf``` to assist in the scripts that handle provisioning the VM. So I went ahead and installed the gems before starting out with the vagrant install.

What I could not understand was how berkshelf which was installed under my rbenv (via homebrew) was not found while the gem was being picked up right! << I know, shitty!

My local setup gave me this:
	
	myfirstvagrantproject $ vagrant version
	Installed Version: 1.6.5
	Latest Version: 1.6.5
	You're running an up-to-date version of Vagrant!

	myfirstvagrantproject $ berks --version
	3.1.5
	myfirstvagrantproject $ which berks
	/Users/cruisemaniac/.rbenv/shims/berks
	
All is well right? Well, No!

Apparently, the berkshelf gem used by vagrant requries [chefdk][1], the chef development kit and wont work hanky panky without that.

> Remove any berkshelf AND chef gems from your ruby installationbefore you start off with the chefdk installation.

Install chefdk into your ```/opt``` folder - again a requirement. Add this to the ```.bash_profile``` before your gems are loaded.

	export PATH=/opt/chefdk/bin
	
Reload your ```.bash_profile``` and you must be good to go.

The other option is that you dont use berkshelf with chef OR you use puppet.

If you dont want to use berkshelf, open the ```Vagrantfile``` and set the following to false:

	Vagrant.configure("2") do |config|
	  config.berkshelf.enabled = false
	  ...
	  # other configuration code
	  
You would also have to remove the ```Berkshelf``` file sitting in your vagrant directory to prevent Vagrant from complaining about an existing Berkshelf file that its not using!

Hope this helps.

[1]: https://downloads.getchef.com/chef-dk/