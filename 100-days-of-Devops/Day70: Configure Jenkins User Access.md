The Nautilus team is integrating Jenkins into their CI/CD pipelines. After setting up a new Jenkins server, they're now configuring user access for the development team, Follow these steps:



1. Click on the Jenkins button on the top bar to access the Jenkins UI. Login with username admin and password Adm!n321.

2. Create a jenkins user named jim with the passwordGyQkFRVNr3. Their full name should match Jim.

3. Utilize the Project-based Matrix Authorization Strategy to assign overall read permission to the jim user.

4. Remove all permissions for Anonymous users (if any) ensuring that the admin user retains overall Administer permissions.

5. For the existing job, grant jim user only read permissions, disregarding other permissions such as Agent, SCM etc.


Note:

1. You may need to install plugins and restart Jenkins service. After plugins installation, select Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page.


2. After restarting the Jenkins service, wait for the Jenkins login page to reappear before proceeding. Avoid clicking Finish immediately after restarting the service.


3. Capture screenshots of your configuration for review purposes. Consider using screen recording software like loom.com for documentation and sharing.

## Solution

Here’s a **clear step-by-step guide** to perform the Jenkins user configuration task as described:

---

### 🧩 Step 1: Login to Jenkins

1. Open your Jenkins web console (e.g., `http://<jenkins-server>:8080`).
2. Click on **“Jenkins”** (top left corner) to go to the main dashboard.
3. Login with:

   * **Username:** `admin`
   * **Password:** `Adm!n321`

---

### ⚙️ Step 2: Create a New User “jim”

1. Go to **Manage Jenkins → Security → Manage Users.**
2. Click **Create User**.
3. Fill in details:

   * **Username:** `jim`
   * **Password:** `GyQkFRVNr3`
   * **Full Name:** `Jim`
   * **Email:** (optional)
4. Click **Create User**.

---

### 🔒 Step 3: Configure Authorization Strategy

1. Go to **Manage Jenkins → Security → Configure Global Security.**

2. Under **Authorization**, select:

   * **“Project-based Matrix Authorization Strategy”**
     (If not available, go to *Manage Plugins → Available → search for “Matrix Authorization Strategy” → Install → Restart Jenkins when installation completes*.)

3. Now you’ll see a permissions matrix table.

---

### ✅ Step 4: Set Permissions

1. In the matrix:

   * Click **Add user or group**, enter **`jim`**, then click **OK**.
   * Under the **Overall** column, check only **Read** for `jim`.
   * Ensure **admin** user retains **Overall → Administer** (and all other permissions if already checked).
   * Remove or uncheck all permissions for **Anonymous User**.

2. Click **Save**.

---

### 🧱 Step 5: Set Job-Specific Permissions for “jim”

1. Go to the Jenkins dashboard → open the existing job.
2. Click **Configure**.
3. Scroll down to **“Project-based Matrix Authorization Strategy”** (if not visible, enable it under *Configure Global Security*).
4. Add user **jim**, then:

   * Give **Read** permission under **Job**.
   * Do **not** assign any other permissions (like Build, Configure, SCM, etc.).
5. Click **Save**.

---

### 🔁 Step 6: Plugin Installation & Restart (If Required)

If Jenkins prompts for a plugin installation:

1. Go to **Manage Jenkins → Manage Plugins → Available tab.**
2. Search and install **Matrix Authorization Strategy Plugin**.
3. Select **Restart Jenkins when installation is complete and no jobs are running.**
4. Wait for Jenkins to restart fully until the **login page** reappears.
5. Log in again with `admin` to continue configuration.

---

### 🖼️ Step 7: Documentation (Screenshots/Recording)

Take screenshots or record using **Loom**:

* Jenkins dashboard (after login)
* User creation page
* Matrix authorization configuration
* Job-specific permission configuration
* Final permission matrix showing only admin and jim.

---

Would you like me to provide a **sample screenshot reference layout (like how the permission matrix should look)** for documentation or review?


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/853de6df-f477-4471-ad74-2cae7f31774f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9a4841e6-0120-4ac8-91e0-4cd244493085" />


```

```
