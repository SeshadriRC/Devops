During the weekly meeting, the Nautilus DevOps team discussed about the automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with Ansible for now due to its simple setup and minimal pre-requisites. The team wanted to start testing using Ansible, so they have decided to use jump host as an Ansible controller to test different kind of tasks on rest of the servers.



Install ansible version 4.7.0 on Jump host using pip3 only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

```
python3 -m pip -V  --> we are checking whether pip is available or not
python3 -m pip install --upgrade pip ( upgrade the pip verson )
python3 -m pip install  ansible==4.7.0  ( install the ansible )
ansible --version
```
