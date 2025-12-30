The Nautilus development team has initiated a new project development, establishing various Git repositories to manage each project's source code. Recently, a repository named /opt/games.git was created. The team has provided a sample index.html file located on the jump host under the /tmp directory. This repository has been cloned to /usr/src/kodekloudrepos on the storage server in the Stratos DC.



Copy the sample index.html file from the jump host to the storage server placing it within the cloned repository at /usr/src/kodekloudrepos/games.

Add and commit the file to the repository.

Push the changes to the master branch.

## Solution

```
[root@ststor01 ~]# ls -lrth /usr/src/kodekloudrepos/games/
total 12K
-rw-r--r-- 1 root root 26 Dec 30 13:23 welcome.txt
-rw-r--r-- 1 root root 34 Dec 30 13:23 info.txt
-rw-r--r-- 1 root root 27 Dec 30 13:34 index.html

thor@jumphost /tmp$ scp index.html natasha@ststor01.stratos.xfusioncorp.com:/tmp
natasha@ststor01.stratos.xfusioncorp.com's password: 
index.html                                             100%   27   110.0KB/s   00:00    
thor@jumphost /tmp$

[root@ststor01 ~]# cd /usr/src/kodekloudrepos/games/
[root@ststor01 games]# ls
index.html  info.txt  welcome.txt
[root@ststor01 games]# git status

git add .
git commit -m "msg"
git push
```
