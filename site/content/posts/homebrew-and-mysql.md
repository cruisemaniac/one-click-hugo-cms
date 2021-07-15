---
comments: true
date: "2014-09-05T17:00:00Z"
description: Installing MySQL via Homebrew
tags:
- tech
- mysql
- homebrew
- troubleshooting
title: Homebrew and MySQL
---

I was recently trying to setup MySQL with [Homebrew][1] and noticed that the mysql server that brew installed was not playing nice with the existing setups.

The solution is to wipe out any existing MySQL installs you've got on your mac and redo everything from scratch with brew. This makes the mysql installed from Brew your main installation.

Read up on this [blogpost][2] that details the sequence. 5 minutes, a few commands and all is hanky-panky in mysql land :-)

Credits to [Cory Simmons][3] for the original blog post!

[1]: http://brew.sh  "Homebrew"
[2]: https://coderwall.com/p/os6woq "Corey Simmons - Uninstall mySQL and start from scratch with HomeBrew"
[3]: https://coderwall.com/corysimmons "Cory Simmons - Assembly.com"