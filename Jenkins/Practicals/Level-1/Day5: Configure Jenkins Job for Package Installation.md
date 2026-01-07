Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:



1. Access the Jenkins UI by clicking on the Jenkins button in the top bar. Log in using the credentials: username admin and password Adm!n321.


2. Create a new Jenkins job named install-packages and configure it with the following specifications:


Add a string parameter named PACKAGE.
Configure the job to install a package specified in the $PACKAGE parameter on the storage server within the Stratos Datacenter.

Note:


1. Ensure to install any required plugins and restart the Jenkins service if necessary. Opt for Restart Jenkins when installation is complete and no jobs are running on the plugin installation/update page. Refresh the UI page if needed after restarting the service.


2. Verify that the Jenkins job runs successfully on repeated executions to ensure reliability.


3. Capture screenshots of your configuration for documentation and review purposes. Alternatively, use screen recording software like loom.com for comprehensive documentation and sharing.

## Solution

Below are the **exact steps** you need to follow in the Nautilus Jenkins lab. This is a **Freestyle Jenkins job** using a **string parameter** and a **shell command** to install a package on the **storage server**.

---

## Step 1: Login to Jenkins

1. Click on the **Jenkins** button in the top bar.
2. Login with:

   * **Username:** `admin`
   * **Password:** `Adm!n321`

---

## Step 2: Create a New Jenkins Job

1. Click **New Item**
2. Enter job name:

   ```
   install-packages
   ```
3. Select **Freestyle project**
4. Click **OK**

---

## Step 3: Add String Parameter

1. In job configuration, scroll to **This project is parameterized**
2. Check the box ✅
3. Click **Add Parameter → String Parameter**
4. Fill the details:

   * **Name:** `PACKAGE`
   * **Default Value:** *(leave empty or optional)*
   * **Description:** `Package name to install`

---

## Step 4: Configure Package Installation on Storage Server

1. Scroll to **Build**
2. Click **Add build step → Execute shell**
3. Add the following command:

```bash
ssh storage-server "sudo yum install -y $PACKAGE"
```

📌 **Explanation:**

* `storage-server` → hostname of the storage server in Stratos DC
* `$PACKAGE` → Jenkins string parameter
* `yum install -y` → installs package without confirmation

*(Storage servers in Nautilus labs are typically CentOS/RHEL-based)*

---

## Step 5: Save the Job

Click **Save**

---

## Step 6: Test the Job

1. Click **Build with Parameters**
2. Enter a package name, for example:

   ```
   httpd
   ```
3. Click **Build**
4. Check **Console Output** → package should install successfully

---

## ✅ Final Outcome

* Jenkins job **install-packages** created
* Accepts package name dynamically
* Installs package on **storage server**
* Fully automated via Jenkins

If you want, I can also explain:

* How SSH works here
* Why `$PACKAGE` expands correctly
* Or how to modify this for `apt` systems


---

<img width="1919" height="626" alt="image" src="https://github.com/user-attachments/assets/e4035458-4645-42bd-8a52-d52370933688" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ce10ae29-6bdc-482b-89a2-11ab237aa622" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ed2d9e4a-09aa-4a3e-b5d9-71bfeb41315b" />

- installed ssh and ssh creds plugin

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e8286697-2657-4c90-b4fa-a5dfee8fbd3a" />

- add creds

<img width="1919" height="483" alt="image" src="https://github.com/user-attachments/assets/d44d3c78-0fea-4bc1-9b44-1afe99d73b13" />

<img width="1919" height="816" alt="image" src="https://github.com/user-attachments/assets/f8492d7b-ee23-4b5d-a7eb-74092d1383c4" />

<img width="1559" height="683" alt="image" src="https://github.com/user-attachments/assets/aff70b76-25eb-4b48-93ae-cf96d5970def" />

