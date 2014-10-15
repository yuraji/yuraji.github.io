---
layout: page
title: Git
---




### # hook @hub -> update production files

When editing the project locally and pushing the commit to hub, you want the hub to automatically update the repository with production files.

`~/git/projectname.git/hooks/post-update`

	#!/bin/sh
	echo; echo "**** Pulling changes into Web [Hub's post-update hook]"; echo;
	
	cd ~/project || exit
	
	unset GIT_DIR
	git pull hub master
	exec git-update-server-info


---------------------------------------------------------------------------


### # hook @production files commit -> update hub

When editing the project on server, you want the commit to automatically be pushed to projects hub.

`~/project/.git/hooks/post-commit`


	#!/bin/sh
	echo; echo "**** pushing changes to Hub [Web's post-commit hook]"; echo;
	git push hub











# Git Resources

[http://gitready.com/](http://gitready.com/)

[http://gitguru.com/](http://gitguru.com/)