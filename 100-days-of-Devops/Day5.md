Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 1 in the Stratos Datacenter:

Install the required SELinux packages.

Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.

No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.

Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.

## Solution
```
Login as root
yum -y install selinux*
sudo apt-get update
sudo apt-get install -y selinux selinux-basics selinux-policy-default auditd
yum list available selinux\* -> list the available packages
rpm -qa | grep selinux -> list the installed packages
yum -y install selinux* --allowerasing
```
