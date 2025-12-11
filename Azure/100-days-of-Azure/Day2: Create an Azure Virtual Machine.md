The Nautilus DevOps team is planning to migrate a portion of their infrastructure to the Azure cloud incrementally. As part of this migration, you are tasked with creating an Azure Virtual Machine (VM).

The requirements are:

1) Use the existing resource group.

2) The VM name must be datacenter-vm, it should be in West US region.

3) Use the Ubuntu 22.04 LTS image for the VM.

4) The VM size must be Standard_B1s.

5) Attach a default Network Security Group (NSG) that allows inbound SSH (port 22).

6) Attach a 30 GB storage disk of type Standard HDD.

7) The rest of the configurations should remain as default.

After completing these steps, make sure you can SSH into the virtual machine.


Use below given Azure Credentials: (You can run the showcreds command on the azure-client host to retrieve credentials)



<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/434e8057-a45a-40da-80cf-8f2ea6cb476c" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/639c2cb3-5f47-4fd1-a14a-b35775adf999" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9e705d4a-fcf8-4095-91eb-3365eace3c18" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/87add379-30a3-4db2-81e2-824a305649ca" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5914f42b-6240-4149-b178-6f5abb270ed8" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/22b4aeea-e562-4d85-8f8f-79350da1fe2d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f9ec56c2-ea1b-4ab7-86ec-aec9c8f663e4" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/feb30d63-d0a7-4201-bc3e-76e51c9c04c4" />

Below are the **complete steps** to create the Azure VM **both in the Azure Portal (Console)** and **Azure CLI**, exactly matching your requirements.

---

# ✅ **PART 1: Azure Portal (Console) Steps**

### **Step 1: Log in to Azure Portal**

Go to: [https://portal.azure.com](https://portal.azure.com)

---

### **Step 2: Go to Resource Groups**

* Search for **Resource Groups**
* Click on the **existing resource group** (as per requirement)

---

### **Step 3: Create Virtual Machine**

Inside the resource group:

* Click **Create → Virtual Machine**

---

### **Step 4: Basics Tab Configuration**

Fill in the following:

| Field                    | Value                                  |
| ------------------------ | -------------------------------------- |
| **Subscription**         | Default                                |
| **Resource Group**       | Select existing                        |
| **Virtual machine name** | `datacenter-vm`                        |
| **Region**               | **West US**                            |
| **Image**                | **Ubuntu Server 22.04 LTS – x64 Gen2** |
| **Size**                 | Standard_B1s                           |

---

### **Step 5: Administrator Account**

* Choose **SSH public key**
* Create or upload your SSH key

---

### **Step 6: Networking Tab**

* A default NSG will be created automatically
* Ensure **SSH (22)** inbound rule is checked

✔ This satisfies requirement #5.

---

### **Step 7: Disks Tab**

Change OS Disk settings:

* **OS Disk Type** → **Standard HDD**
* Size → **30 GiB**

---

### **Step 8: Review + Create**

* Click **Review + Create**
* Wait for validation
* Click **Create**

Your VM `datacenter-vm` in **West US** is now deployed.

---

# ✅ **PART 2: Azure CLI Steps**

> Make sure you are logged in:

```bash
az login
```

And select the correct subscription if needed:

```bash
az account set --subscription "<SUBSCRIPTION_ID>"
```

---

# ✔ **Step 1: Variables (optional but clean)**

```bash
RG="kml_rg_main-ddf1062158e543a8"
VM="datacenter2"
LOC="westus"
IMAGE="Ubuntu2204"
SIZE="Standard_B1s"
DISK=30
```

Replace `your-existing-rg` with your real resource group.

---

# ✔ **Step 2: Create VM**

This single command will:

* Create NIC
* Create NSG
* Allow SSH (22)
* Create Public IP
* Create 30 GB Standard HDD OS disk
* Create the VM

```bash
az vm create \
  --resource-group $RG \
  --name $VM \
  --image Ubuntu2204 \
  --size $SIZE \
  --location $LOC \
  --os-disk-size-gb $DISK \
  --storage-sku Standard_LRS \
  --admin-username azureuser \
  --generate-ssh-keys \
  --nsg-rule ssh

```

---

# ✔ **Confirm VM Created**

```bash
az vm show --resource-group $RG --name $VM
```

---

# ✔ **Check NSG Rule**

```bash
az network nsg list --resource-group $RG --output table
```

---

# ✔ **Check OS Disk size**

```bash
az vm show \
  --resource-group $RG \
  --name $VM \
  --query "storageProfile.osDisk.diskSizeGb"
```

---

# 🎉 **Your VM with all requirements is ready.**

If you want, I can also provide:

* Terraform version
* ARM template version
* Bicep version

Just tell me!

