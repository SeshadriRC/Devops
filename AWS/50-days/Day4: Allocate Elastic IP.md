The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, allocate an Elastic IP address, name it as nautilus-eip.


## Solution

To complete this task in AWS, you need to **allocate an Elastic IP (EIP)** and **tag/name it as `nautilus-eip`**.

Below are **clear steps**, first via **AWS Console** and then **AWS CLI** (preferred in DevOps labs).

---

## ✅ Option 1: AWS Management Console

1. Log in to **AWS Console**
2. Go to **EC2**
3. In the left menu, click **Elastic IPs**
4. Click **Allocate Elastic IP address**
5. Keep defaults:

   * Public IPv4 address pool: **Amazon**
6. Click **Allocate**
7. Select the newly created Elastic IP
8. Click **Actions → Manage tags**
9. Add tag:

   * **Key:** `Name`
   * **Value:** `nautilus-eip`
10. Click **Save**

✅ Elastic IP allocated and named successfully.

---

## ✅ Option 2: AWS CLI (Recommended)

### 1️⃣ Allocate Elastic IP

```bash
aws ec2 allocate-address --domain vpc
```

This will return output like:

```json
{
  "PublicIp": "3.x.x.x",
  "AllocationId": "eipalloc-0abc123xyz"
}
```

---

### 2️⃣ Add Name tag

```bash
aws ec2 create-tags \
  --resources eipalloc-0abc123xyz \
  --tags Key=Name,Value=nautilus-eip
```

---

### 3️⃣ Verify

```bash
aws ec2 describe-addresses --allocation-ids eipalloc-0abc123xyz
```

---

## 🧠 Key Notes (Interview / Real-world)

* Elastic IPs are **static public IPv4 addresses**
* They remain allocated until explicitly released
* **Charges apply** if EIP is not associated with a running resource
* Commonly attached to **EC2, NAT Gateway, Load Balancer**

---

If you want, I can also show:

* How to **attach this EIP to an EC2**
* How to **release EIP safely**
* Terraform version of this task

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/99a76af3-c4d4-4166-abbe-4d32524c5b0c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/35696487-0c58-4cb4-81bf-523453c846de" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/35a7c5e9-3e3e-4b38-bf5a-f5f7ca75329a" />

