---
layout: page
title: Git
---




### hook @hub -> update production files

When editing the project locally and pushing the commit to hub, you want the hub to automatically update the repository with production files.

~/git/projectname.git/hooks/**post-update**

	#!/bin/sh
	echo; echo "**** Pulling changes into Web [Hub's post-update hook]"; echo;
	
	cd ~/project || exit
	
	unset GIT_DIR
	git pull hub master
	exec git-update-server-info


----


### hook @production files commit -> update hub

When editing the project on server, you want the commit to automatically be pushed to projects hub.

~/project/.git/hooks/**post-commit**


	#!/bin/sh
	echo; echo "**** pushing changes to Hub [Web's post-commit hook]"; echo;
	git push hub


----


### Setup


	git config --global user.name 'Yuriy Linnyk'
	git config --global user.email yuraji@gmail.com
	git config --global color.ui true
	
	git config --global alias.st status
	git config --global alias.br branch
	git config --global alias.c "commit -m"
	git config --global alias.a "add -A ."
	git config --global alias.co checkout
	git config --global alias.ls "ls-files"
	git config --global alias.lol "log --graph --decorate --pretty=oneline --abbrev-commit"
	git config --global alias.lola "log --graph --decorate --pretty=oneline --abbrev-commit --all"
	git config --global alias.ign "ls-files -o -i --exclude-standard"







### Git Resources


[cheatsheet](http://cheat.errtheblog.com/s/git)

[http://gitready.com/](http://gitready.com/)

[http://gitguru.com/](http://gitguru.com/)