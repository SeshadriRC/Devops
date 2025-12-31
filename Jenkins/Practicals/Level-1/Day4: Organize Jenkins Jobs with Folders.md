xFusionCorp Industries' DevOps team aims to streamline the management of Jenkins jobs by organizing them into distinct folders based on their purpose. Complete the task following the provided requirements:



1.Access the Jenkins UI by clicking on the Jenkins button in the top bar. Log in using the credentials: username admin and password Adm!n321.

2. Create a new folder named Apache within the Jenkins UI.

3. Move the existing jobs httpd-php and services under the newly created Apache folder.

Note:

1. Ensure to install any required plugins and restart the Jenkins service if necessary. Opt for Restart Jenkins when installation is complete and no jobs are running on the plugin installation/update page.

2. Be aware that Jenkins UI may experience temporary unresponsiveness during the service restart. Refresh the UI page if needed.

3. Capture screenshots of your work for documentation and review purposes. Alternatively, utilize screen recording software like loom.com for detailed documentation and sharing.


## Solution

Here’s a clear, **step-by-step guide** to complete the task in Jenkins UI.

---

## Step 1: Access Jenkins UI

1. Click the **Jenkins** button from the top bar.
2. Log in with:

   * **Username:** `admin`
   * **Password:** `Adm!n321`

---

## Step 2: Ensure Required Plugin (Folders Plugin)

To create folders, Jenkins needs the **Folders Plugin**.

1. Go to **Manage Jenkins → Manage Plugins**
2. Open the **Available** tab
3. Search for **Folders**
4. If not installed:

   * Select **Folders Plugin**
   * Click **Install without restart** or **Install and Restart**
5. Choose **Restart Jenkins when installation is complete and no jobs are running**
6. After restart, refresh the Jenkins UI if it becomes unresponsive

---

## Step 3: Create a New Folder Named `Apache`

1. From the Jenkins dashboard, click **New Item**
2. Enter name: **Apache**
3. Select **Folder**
4. Click **OK**
5. Leave default settings (or add description if required)
6. Click **Save**

---

## Step 4: Move Jobs into the Apache Folder

You need to move these jobs:

* `httpd-php`
* `services`

### Method (Recommended):

1. Open the **Apache** folder
2. Click **Move** (or **Add items** depending on UI)
3. Select:

   * `httpd-php`
   * `services`
4. Confirm the move

### Alternative Method:

1. Open each job individually
2. Click **Move**
3. Select destination folder: **Apache**
4. Confirm

---

## Step 5: Verify

1. Open the **Apache** folder
2. Confirm both jobs are present:

   * ✔ httpd-php
   * ✔ services
3. Ensure no duplicate jobs remain at the root level

---

## Step 6: Documentation (Mandatory)

* Capture **screenshots** of:

  * Apache folder creation
  * Jobs inside the Apache folder
* Or record a **screen capture** (e.g., using **loom.com**) showing:

  * Plugin check
  * Folder creation
  * Job movement

---

### ✅ Final Outcome

* Folder **Apache** created
* Jobs **httpd-php** and **services** successfully organized under it
* Jenkins structure is clean and purpose-based

If you want, I can also provide **exact screenshot checkpoints** or **common issues during job move**.

---

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eb4655ac-bcb9-4e77-835c-245ccd488ae2" />
<img width="1914" height="468" alt="image" src="https://github.com/user-attachments/assets/e549e9c6-6e15-49f5-bd5d-0e3e11b295f2" />
<img width="1919" height="910" alt="image" src="https://github.com/user-attachments/assets/79ad44f6-30c2-4a35-afe0-e220332197aa" />

- Leave description as it is
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/391f8f90-5d93-4e89-8f90-3b9811cae880" />

- go to job and move it

<img width="1919" height="602" alt="image" src="https://github.com/user-attachments/assets/85768d68-d362-4b31-ba66-ee8d0ec7332b" />
<img width="1919" height="488" alt="image" src="https://github.com/user-attachments/assets/a967fce1-968f-4cff-a6c5-9e8f590dba20" />
<img width="1897" height="797" alt="image" src="https://github.com/user-attachments/assets/82a30ee8-0ff5-4d49-909a-fd07792db771" />
