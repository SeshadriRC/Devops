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

- https://www.nbtechsupport.co.in/2023/09/jenkins-install-packages-kodekloud-add.html

```
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/17e856ef-92e5-4d1c-a56f-70c217aa16f0" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/511bd3cd-c780-4f48-8cc4-47cecd1870f7" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ac3ff575-98da-4219-8f04-43a607711553" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/98645bb4-b246-4d23-b5df-657f01de4b12" />



Here’s how you can **complete the “install-packages” Jenkins job setup step by step**, following the given Nautilus task instructions:

---

### 🧩 Step 1: Access Jenkins

1. Click on the **“Jenkins”** button on the top bar (in the lab interface).
2. Log in using the credentials:

   ```
   Username: admin
   Password: Adm!n321
   ```

---

### ⚙️ Step 2: Create a New Jenkins Job

1. Click **“New Item”** on the Jenkins dashboard.
2. Enter **`install-packages`** as the job name.
3. Select **“Freestyle project”** → Click **OK**.

---

### 🧱 Step 3: Add a Parameter (PACKAGE)

1. In the job configuration page:

   * Scroll down to **“This project is parameterized”** → check the box.
   * Click **“Add Parameter” → “String Parameter.”**
   * Set:

     * **Name:** `PACKAGE`
     * **Default Value:** `httpd` *(for testing)*
     * **Description:** `Enter the name of the package to install on storage server.`

---

### 🖥️ Step 4: Add Build Step to Install Package

You’ll now make Jenkins run a command to install the specified package on the **storage server**.

There are two common ways — depending on what’s allowed in your Jenkins setup.

#### ✅ Option A: Using SSH (Recommended)

1. Ensure the **SSH Agent Plugin** or **Publish Over SSH Plugin** is installed.

   * Go to **Manage Jenkins → Manage Plugins → Available tab**
   * Search and install **Publish Over SSH**.
   * After installation → choose **“Restart Jenkins when installation is complete and no jobs are running.”**
   * Wait for Jenkins to restart, then refresh the UI.

2. Configure SSH connection:

   * Go to **Manage Jenkins → Configure System → Publish over SSH.**
   * Add a new SSH server:

     * **Name:** `storage-server`
     * **Hostname:** (enter IP or DNS of storage server)
     * **Username:** `root` (or `centos`, as applicable)
     * **Password/Key:** paste credentials as per the environment
   * Click **Test Configuration** → Ensure it’s successful.
   * Click **Save.**

3. Back in the **install-packages** job:

   * Scroll to **Build → Add build step → Send files or execute commands over SSH.**
   * Choose the SSH server: `storage-server`
   * Under **Exec command**, enter:

     ```bash
     sudo yum install -y ${PACKAGE}
     ```
   * (If Debian/Ubuntu, use `sudo apt-get install -y ${PACKAGE}`)

---

#### ✅ Option B: Using “Execute Shell” (if SSH already set up via keys)

If Jenkins user can SSH to the storage server without password:

1. Add a build step → **Execute shell**
2. Enter the following:

   ```bash
   ssh root@ststor01 'yum install -y ${PACKAGE}'
   ```

*(Replace `ststor01` with the actual hostname or IP of the storage server)*

---

### 🔄 Step 5: Save and Test the Job

1. Click **Save**.
2. On the job page → Click **Build with Parameters**.
3. Enter a test value for `PACKAGE` (e.g., `git`).
4. Click **Build**.
5. Open the **Console Output** → Verify that the package installation completed successfully.

---

### 🔁 Step 6: Verify Repeated Executions

Run the job multiple times with different values like:

```
PACKAGE = wget
PACKAGE = net-tools
PACKAGE = curl
```

Each time, confirm the build completes successfully (exit code 0).

---

### 📸 Step 7: Documentation

* Take **screenshots** of:

  * Job configuration page
  * Parameter input screen
  * Console output showing successful installation

OR

* Use **loom.com** or any screen recording tool to record the full process:

  * Logging in
  * Job creation/config
  * Plugin installation
  * Job execution and verification

---

### ✅ Final Verification Checklist

| Item                                   | Status |
| -------------------------------------- | ------ |
| Jenkins job `install-packages` created | ✅      |
| String parameter `PACKAGE` added       | ✅      |
| Command executes on storage server     | ✅      |
| Plugin installed if needed             | ✅      |
| Job runs successfully multiple times   | ✅      |
| Screenshots or recording captured      | ✅      |

---

Would you like me to give you the **exact shell commands** to verify remote package installation (like test connectivity or package presence on the storage server)?

