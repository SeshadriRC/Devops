## Stash and gitignore

 If the file is already tracked, untrack it

.gitignore won’t work if Git is already tracking the file. You need to remove it from the index (not from your disk):

git rm --cached myfile.txt

echo "myfile.txt" >> .gitignore
