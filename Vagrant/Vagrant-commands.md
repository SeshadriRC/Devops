Here’s a **concise list of important and frequently used Vagrant commands**, grouped by purpose 👇

---

### 🧱 **1️⃣ Basic Lifecycle Commands**

| Command              | Description                                                              |
| -------------------- | ------------------------------------------------------------------------ |
| `vagrant init <box>` | Initializes a new Vagrantfile in the current directory using a base box. |
| `vagrant up`         | Creates and starts the VM defined in the Vagrantfile.                    |
| `vagrant halt`       | Gracefully shuts down the running VM.                                    |
| `vagrant reload`     | Restarts the VM and applies changes made to the Vagrantfile.             |
| `vagrant destroy`    | Stops and deletes the VM and all its resources.                          |

---

### 🧰 **2️⃣ Box Management**

| Command                        | Description                                   |
| ------------------------------ | --------------------------------------------- |
| `vagrant box list`             | Lists all downloaded boxes.                   |
| `vagrant box add <name> <url>` | Adds a new base box from a URL or local file. |
| `vagrant box remove <name>`    | Removes a box from your system.               |
| `vagrant box update`           | Updates boxes to the latest version.          |

---

### 💻 **3️⃣ VM Access & Control**

| Command           | Description                            |
| ----------------- | -------------------------------------- |
| `vagrant ssh`     | SSH into the running VM.               |
| `vagrant suspend` | Suspends the VM (saves state to disk). |
| `vagrant resume`  | Resumes a suspended VM.                |
| `vagrant status`  | Displays the current status of the VM. |

---

### ⚙️ **4️⃣ Configuration & Debugging**

| Command                           | Description                                                    |
| --------------------------------- | -------------------------------------------------------------- |
| `vagrant provision`               | Reruns the provisioning scripts (like shell, Ansible, Puppet). |
| `vagrant reload --provision`      | Restarts and reprovisions the VM in one step.                  |
| `vagrant global-status`           | Shows all active Vagrant environments.                         |
| `vagrant ssh-config`              | Outputs SSH configuration details for the VM.                  |
| `vagrant plugin list`             | Lists installed Vagrant plugins.                               |
| `vagrant plugin install <plugin>` | Installs a plugin.                                             |
| `vagrant version`                 | Displays current Vagrant version.                              |

---

### 🧹 **5️⃣ Cleanup & Troubleshooting**

| Command                      | Description                                            |
| ---------------------------- | ------------------------------------------------------ |
| `vagrant destroy -f`         | Force destroys the VM without asking for confirmation. |
| `vagrant up --debug`         | Starts VM with verbose debug logs.                     |
| `vagrant reload --provision` | Applies updated configuration and reprovisions.        |

---

### 🧩 **Example Workflow**

```bash
vagrant init ubuntu/focal64
vagrant up
vagrant ssh
# make some changes
vagrant halt
vagrant destroy
```

---

Would you like me to include **commonly used networking and shared folder settings** in the Vagrantfile (useful for DevOps/k8s practice)?
