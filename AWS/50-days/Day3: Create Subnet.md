The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.

For this task, create one subnet named devops-subnet under default VPC.

## Solution

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e731c79d-4a47-4f72-8270-2aafb8499805" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/73a9526e-a137-410d-a263-99e0def6e3e3" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/40e9a5d6-bdd9-4e5c-92d1-5a876e7afcea" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c05d1858-81c5-4d49-a694-dac0665f0001" />


---
Below are **clear, step-by-step instructions** to create **one subnet named `devops-subnet` under the default VPC**, using **both AWS Console and AWS CLI**.

I’ll assume **no specific CIDR or AZ was mandated** (common in such tasks). I’ll also point out where you can change them if needed.

---

# ✅ Using AWS Management Console

### Step 1: Login to AWS Console

* Go to **AWS Console**
* Select the **correct region** (top-right)

---

### Step 2: Go to VPC Dashboard

* Search for **VPC**
* Click **VPC** → **Subnets**

---

### Step 3: Create subnet

* Click **Create subnet**

---

### Step 4: Subnet configuration

Fill the details:

| Field                 | Value                           |
| --------------------- | ------------------------------- |
| **VPC ID**            | Select **Default VPC**          |
| **Subnet name**       | `devops-subnet`                 |
| **Availability Zone** | Choose any (e.g., `us-east-1a`) |
| **IPv4 CIDR block**   | Example: `172.31.50.0/24`       |

> ⚠️ CIDR must be **within default VPC CIDR** (usually `172.31.0.0/16`)

---

### Step 5: Create

* Click **Create subnet**

✅ Subnet `devops-subnet` is now created under default VPC.

---

# ✅ Using AWS CLI

## Step 1: Identify the Default VPC ID

```bash
aws ec2 describe-vpcs \
  --filters Name=isDefault,Values=true \
  --query "Vpcs[0].VpcId" \
  --output text
```

Example output:

```
vpc-0abc1234def567890
```

---

## Step 2: Choose Availability Zone

```bash
aws ec2 describe-availability-zones \
  --query "AvailabilityZones[*].ZoneName" \
  --output table
```

Example:

```
us-east-1a
```

---

## Step 3: Create the subnet

```bash
aws ec2 create-subnet \
  --vpc-id vpc-0abc1234def567890 \
  --cidr-block 172.31.50.0/24 \
  --availability-zone us-east-1a \
  --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=devops-subnet}]'
```

---

## Step 4: Verify subnet

```bash
aws ec2 describe-subnets \
  --filters Name=tag:Name,Values=devops-subnet \
  --query "Subnets[*].[SubnetId,VpcId,CidrBlock,AvailabilityZone]" \
  --output table
```

---

# 🔍 Summary

| Method      | Result                                       |
| ----------- | -------------------------------------------- |
| AWS Console | Subnet created under default VPC             |
| AWS CLI     | Subnet created with Name tag `devops-subnet` |
| VPC         | Default VPC                                  |
| CIDR        | Any valid range inside default VPC           |

---

## 🧠 Important notes (exam / real-world)

* Default VPC **already exists**
* Subnets **must not overlap**
* Naming is done via **Tags**
* AZ selection is mandatory

If you want, I can also show:

* How to make the subnet **public**
* How to enable **auto-assign public IP**
* Terraform version of the same task

```
