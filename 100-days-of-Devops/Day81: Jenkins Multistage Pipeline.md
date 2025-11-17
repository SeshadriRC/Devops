The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:



Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.


Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123.


There is a repository named sarah/web in Gitea that is already cloned on Storage server under /var/www/html directory.


Update the content of the file index.html under the same repository to Welcome to xFusionCorp Industries and push the changes to the origin into the master branch.


Apache is already installed on all app Servers its running on port 8080.


Create a Jenkins pipeline job named deploy-job (it must not be a Multibranch pipeline job) and pipeline should have two stages Deploy and Test ( names are case sensitive ). Configure these stages as per details mentioned below.


a. The Deploy stage should deploy the code from web repository under /var/www/html on the Storage Server, as this location is already mounted to the document root /var/www/html of all app servers.


b. The Test stage should just test if the app is working fine and website is accessible. Its up to you how you design this stage to test it out, you can simply add a curl command as well to run a curl against the LBR URL (http://stlb01:8091) to see if the website is working or not. Make sure this stage fails in case the website/app is not working or if the Deploy stage fails.


Click on the App button on the top bar to see the latest changes you deployed. Please make sure the required content is loading on the main URL http://stlb01:8091 i.e there should not be a sub-directory like http://stlb01:8091/web etc.


Note:


You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.


For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

https://www.nbtechsupport.co.in/2021/07/jenkins-multi-stage-pipeline.html?m=1

- install Pipeline Git, SSH plugins
- setup git creds
## Solution
Your error is **very simple**:

### Jenkins cannot access `/var/www/html`

because **that path exists only on the Storage Server**,
but your pipeline is running **on Jenkins Master**, not on Storage Server.

That’s why this fails:

```
cd /var/www/html
cd: can't cd to /var/www/html
```

---

# ✅ **Fix: Use SSH to run commands on the Storage Server**

You MUST connect to the Storage Server in the Deploy stage.

Storage server hostname in KodeKloud is:

```
ststor01
```

The user is usually either **natasha** or **tony** depending on the lab.
Check by SSH:

```
ssh natasha@ststor01
```

or

```
ssh tony@ststor01
```

Usually the password is:

```
Ir0nM@n
```

---

# ⭐ **Correct Jenkinsfile (working version)**

Use **SSH inside the pipeline**:

```groovy
pipeline {
    agent any

    stages {

        stage('Deploy') {
            steps {
                echo "Deploying code to Storage Server..."
                sh '''
                    ssh -o StrictHostKeyChecking=no natasha@ststor01 "cd /var/www/html && sudo git pull origin master"
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Testing website..."
                sh '''
                    curl -f http://stlb01:8091 | grep "Welcome to xFusionCorp Industries"
                '''
            }
        }

    }
}
```

---

# ✔ Why this works

### 1. Jenkins runs on Jenkins master

→ cannot access `/var/www/html` (exists only on Storage server)

### 2. Using SSH makes Jenkins run commands **on Storage Server**, not Jenkins master

→ `/var/www/html` exists there
→ NFS sync updates all app servers automatically

### 3. Test stage verifies website via load balancer

---

# 🔑 **Important: Create SSH Credential in Jenkins**

Go to:

```
Manage Jenkins → Credentials → Global → Add Credentials
```

* Type: SSH Username with password
* Username: natasha (or tony)
* Password: Ir0nM@n
* ID: **STOR_SSH**

Then update Jenkinsfile to use credentials:

```groovy
steps {
    sshagent(credentials: ['STOR_SSH']) {
        sh '''
            ssh -o StrictHostKeyChecking=no natasha@ststor01 "cd /var/www/html && sudo git pull origin master"
        '''
    }
}
```

This is the most correct and stable solution for this KodeKloud lab.

---

***Configure ssh connectivity before running the job***
