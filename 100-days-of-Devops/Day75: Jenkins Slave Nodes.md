The Nautilus DevOps team has installed and configured new Jenkins server in Stratos DC which they will use for CI/CD and for some automation tasks. There is a requirement to add all app servers as slave nodes in Jenkins so that they can perform tasks on these servers using Jenkins. Find below more details and accomplish the task accordingly.



Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.


1. Add all app servers as SSH build agent/slave nodes in Jenkins. Slave node name for app server 1, app server 2 and app server 3 must be App_server_1, App_server_2, App_server_3 respectively.


2. Add labels as below:


App_server_1 : stapp01

App_server_2 : stapp02

App_server_3 : stapp03


3. Remote root directory for App_server_1 must be /home/tony/jenkins, for App_server_2 must be /home/steve/jenkins and for App_server_3 must be /home/banner/jenkins.


4. Make sure slave nodes are online and working properly.


Note:

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.


2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

## Solution

- https://www.nbtechsupport.co.in/2021/07/add-slave-nodes-in-jenkins.html

```
1. Install plugins for ssh
2. configure global creds for all app servers
3. Add Slave Nodes
```

**Snips**

***Plugin Install of SSH***
***Credential Setup***
<img width="1876" height="592" alt="image" src="https://github.com/user-attachments/assets/1b0308e4-70b8-4f2b-b8bf-c42dfdce9fd7" />
***Add slave nodes***
Manage-Jenkins --> System-Configuration --> 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0dc3db85-9046-4145-9351-f4eb66b28da1" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/252de74c-a572-4f40-9d6b-530cf2329363" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7541d9e0-e137-48ef-ad2f-9fc993d2241b" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/59c584c1-c2f9-4c5f-899a-928a1a24ba8f" />

- Install Java on all 3 nodes

 - Add ssh configuration for all 3 nodes in system and check the authentication

   <img width="1804" height="799" alt="image" src="https://github.com/user-attachments/assets/d521f4d1-d865-4f00-9382-12011d89dfad" />

 - Build a job

   <img width="1568" height="528" alt="image" src="https://github.com/user-attachments/assets/164c2199-3237-474f-b41b-c1ae1ea00921" />

- Now submit it

  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ffecb70-8b99-4932-b573-551dfe6ab18b" />

