The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:



Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.


Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123. There under user sarah you will find a repository named web_app that is already cloned on Storage server under /var/www/html. sarah is a developer who is working on this repository.


Add a slave node named Storage Server. It should be labeled as ststor01 and its remote root directory should be /var/www/html.


We have already cloned repository on Storage Server under /var/www/html.


Apache is already installed on all app Servers its running on port 8080.


Create a Jenkins pipeline job named nautilus-webapp-job (it must not be a Multibranch pipeline) and configure it to:


Deploy the code from web_app repository under /var/www/html on Storage Server, as this location is already mounted to the document root /var/www/html of app servers. The pipeline should have a single stage named Deploy ( which is case sensitive ) to accomplish the deployment.

LB server is already configured. You should be able to see the latest changes you made by clicking on the App button. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like https://<LBR-URL>/web_app etc.


Note:


You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.


For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


## Solution

- Install plugin (ssh, pipeline job, git)
- Add Creds for git
- Add cred for storge server to configure slave node
- Install java on the storage server  ( sudo yum install -y java )
- Create slave node and launch aged
- change permissions ( /var/www/html )


  <img width="1915" height="489" alt="image" src="https://github.com/user-attachments/assets/7d45a2c8-2e4f-4740-a3b6-564e9412ddf3" />
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8d937b5f-f156-4f91-a216-2a149ea53531" />

Alright Seshadri, let me break this down cleanly so you can execute the task without losing time. I’ll keep it crisp and practical — exactly the way you prefer when working through Jenkins tasks.

---

# ✅ **High-Level Steps to Complete the Task**

## **1. Add the Storage Server as a Jenkins Slave (Agent)**

Go to **Manage Jenkins → Nodes → New Node**
Create:

* **Node name:** `Storage Server`
* **Type:** Permanent agent
* **Labels:** `ststor01`
* **Remote root directory:** `/var/www/html`
* **Launch method:** SSH (use the given Storage server IP and SSH credentials)

> After saving, make sure the node comes online.
> If you get SSH errors, install **SSH Build Agents plugin** and restart Jenkins.

---

## **2. Verify Gitea Repo on Storage Server**

You don’t clone anything via Jenkins because the repo is **already cloned** on the Storage Server at:
`/var/www/html/web_app`

Apache on App servers already serves content from `/var/www/html/`, so this location is effectively the live deployment path.

---

## **3. Create Jenkins Pipeline Job**

Go to:
**New Item → Pipeline → Name: `nautilus-webapp-job` (NOT multi-branch)**

---

## **4. Configure Pipeline Script**

### ✅ **You must use only one stage named exactly `Deploy`**

This stage will:

* Go to the Storage Server
* Pull the latest changes from Gitea
* Copy the content directly to `/var/www/html/` root
* Ensure there is no `/web_app` subdirectory in final path

---

### ✅ **Pipeline Script (Paste this in the Pipeline script box)**

```groovy
pipeline {
    agent { label 'ststor01' }

    stages {
        stage('Deploy') {
            steps {
                script {
                    sh """
                        cd /var/www/html/web_app
                        git pull

                        # Copy content to document root (without web_app folder)
                        cp -r /var/www/html/web_app/* /var/www/html/

                        echo "Deployment Completed"
                    """
                }
            }
        }
    }
}
```

### Why this works:

* Code is already cloned → just `git pull`
* Apache's document root is already mapped to `/var/www/html` across app servers
* So copying contents **inside** `web_app` → **into** `/var/www/html/` ensures no unwanted path like `/web_app` appears in the URL

---

## ✅ **5. Run the Pipeline**

Click **Build Now** → wait for the Deploy stage to complete.

---

## ✅ **6. Validate Application**

Open the **App** button in the lab → it hits the load balancer.

✔ You must see the website directly at:
`https://<LBR-URL>`

❌ It should NOT load at:
`https://<LBR-URL>/web_app`

---

# ✅ Common Issues & Fixes

### **❌ Pipeline fails to connect to Storage Server**

→ Install plugin: **SSH Build Agents**
→ Restart Jenkins using “Restart Jenkins when installation is complete”

---

### **❌ Jenkins UI frozen after restart**

→ Just refresh the browser.

---

### **❌ Files still showing old content**

→ You may need to run:

```sh
sudo chown -R jenkins:jenkins /var/www/html
```

on the Storage Server.

---

If you want, I can also give you a quick **interview-style explanation** of what's happening behind the scenes with this Jenkins pipeline + agent setup.

