The Nautilus DevOps team is in the process of migrating some of their workloads to Azure. One of the tasks involves creating a new Virtual Machine (VM) using the Azure CLI. The team does not have access to the Azure portal but can manage Azure resources via the azure-client host (the landing host for this lab).

1) Create a new Azure Virtual Machine named devops-vm using the Azure CLI.

2) Use the Ubuntu2204 image and set the VM size to Standard_B2s.

3) Make sure the admin username is set to azureuser and SSH keys are generated for secure access.

4) Use Standard_LRS storage account, disk size must be 30GB and ensure the VM devops-vm is in the running state after creation.


## Solution

Perfect — this is a **very common real-world scenario**. You **do NOT need Azure Portal or console access**. Azure CLI is enough.

I’ll explain this **step by step**, assuming:

* VM already exists: `devops-vm`
* SSH key pair already exists on your machine
* You don’t know the IP address

---

## Step 1️⃣ Find the VM’s Public IP (MOST IMPORTANT)

Run this on the **azure-client host**:

```bash
az vm show \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name devops-vm \
  --show-details \
  --query "publicIps" \
  --output tsv
```

✅ Example output:

```
20.198.45.123
```

👉 This is the **public IP** you’ll SSH into.

---

### 🔹 If public IP is empty

That means:

* VM was created **without a public IP**
* OR NSG blocks SSH

Check NIC details:

```bash
az vm show \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name devops-vm \
  --query "networkProfile.networkInterfaces[0].id" \
  --output tsv
```

Then inspect the NIC’s public IP (tell me if this happens — I’ll guide you).

---

## Step 2️⃣ Ensure SSH (Port 22) is allowed

Check NSG rules:

```bash
az vm open-port \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name devops-vm \
  --port 22
```

This safely ensures:

* TCP 22 is open
* No portal needed

---

## Step 3️⃣ SSH into the VM using your existing key

Assume:

* Username: `azureuser`
* Private key location: `/home/user/.ssh/id_rsa` (example)

Run:

```bash
ssh -i /path/to/your/private_key azureuser@<PUBLIC_IP>
```

Example:

```bash
ssh -i ~/.ssh/id_rsa azureuser@20.198.45.123
```

---

## Step 4️⃣ If permission denied (common issue)

Fix key permissions:

```bash
chmod 600 /path/to/private_key
```

Then retry SSH.

---

## Step 5️⃣ Verify you’re inside the VM

Once logged in:

```bash
hostname
```

Expected output:

```
devops-vm
```

---

## Important notes (Azure-specific)

* Azure **does NOT allow password SSH by default**
* SSH works only with:

  * `--generate-ssh-keys`
  * OR `--ssh-key-values`
* Console access is optional, **not required**

---

## Interview-ready explanation ⭐

> Even without portal or console access, we can retrieve a VM’s public IP using `az vm show --show-details` and connect securely via SSH using the private key. Azure CLI provides full VM management capability.

---

## If you want next, I can help you with:

* SSH when **no public IP exists** (jumpbox / bastion style)
* Add your existing public key to VM
* Reset SSH access via Azure CLI
* Copy files using `scp`
* Stop/start VM via CLI

Just tell me what you want next 👍
