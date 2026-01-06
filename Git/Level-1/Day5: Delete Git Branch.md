The Nautilus developers are engaged in active development on one of the project repositories located at /usr/src/kodekloudrepos/beta. During testing, several test branches were created, and now they require cleanup. Here are the requirements provided to the DevOps team:



On the Storage server in Stratos DC, delete a branch named xfusioncorp_beta from the /usr/src/kodekloudrepos/beta Git repository.

## Solution

```
git branch -d xfusioncorp_beta
```
