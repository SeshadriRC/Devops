The production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on App Server 1 in Stratos Datacenter, and they need to create a bash script named blog_backup.sh which should accomplish the following tasks. (Also remember to place the script under /scripts directory on App Server 1).



a. Create a zip archive named xfusioncorp_blog.zip of /var/www/html/blog directory.


b. Save the archive in /backup/ on App Server 1. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on Nautilus Backup Server.


c. Copy the created archive to Nautilus Backup Server server in /backup/ location.


d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, tony in case of App Server 1) must be able to run it.


e. Do not use sudo inside the script.

Note:
The zip package must be installed on given App Server before executing the script. This package is essential for creating the zip archive of the website files. Install it manually outside the script.

## Solution

```
cd /scripts/
yum install zip

vi blog_backup.sh
#!/bin/bash
zip -r /backup/xfusioncorp_blog.zip /var/www/html/blog
scp /backup/xfusioncorp_blog.zip clint@stbkp01.stratos.xfusioncorp.com:/backup


unzip -l xfusioncorp_blog.zip    ( will show the list of files )


ssh-keygen
ssh-copy-id clint@stbkp01

sh blog_backup.sh
```
