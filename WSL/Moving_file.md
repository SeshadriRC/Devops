```
wsl --list --verbose
wsl --export Ubuntu-24.04 E:\Backup\ubuntu-24.04-backup.tar
wsl --unregister Ubuntu-24.04
wsl --import Ubuntu-24.04 E:\WSL\Ubuntu-24.04 E:\WSL\Backup\ubuntu-24.04-backup.tar --version 2
wsl -d Ubuntu-24.04
```
