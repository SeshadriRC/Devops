The Nautilus application development team was working on a git repository /opt/official.git which is cloned under /usr/src/kodekloudrepos directory present on Storage server in Stratos DC. The team want to setup a hook on this repository, please find below more details:



Merge the feature branch into the master branch`, but before pushing your changes complete below point.

Create a post-update hook in this git repository so that whenever any changes are pushed to the master branch, it creates a release tag with name release-2023-06-15, where 2023-06-15 is supposed to be the current date. For example if today is 20th June, 2023 then the release tag must be release-2023-06-20. Make sure you test the hook at least once and create a release tag for today's release.

Finally remember to push your changes.
Note: Perform this task using the natasha user, and ensure the repository or existing directory permissions are not altered.

## Solution

```
cd to bare repo
/opt/cluster.git/hooks

script
#!/bin/bash
current_date=$(date +"%Y-%m-%d")
release_name="release-$current_date"
git tag $release_name

git checkout master
git merge

push it to remote

git fetch --tags
git tag -l

```

output
```
[natasha@ststor01 official]$ git fetch --tags
From /opt/official
 * [new tag]         release-2025-09-28 -> release-2025-09-28
[natasha@ststor01 official]$ git tag -l
release-2025-09-28
```


Then create the post-update hook
#!/bin/bash

#Get the current date

current_date=$(date +"%Y-%m-%d")

#Create a release tag with the current date

git tag release-$date

Once the tag is created go to cloned repo
Git checkout master

Git merge feature

You will see the tag has been release with current date.
