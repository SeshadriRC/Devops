The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, create a security group under default VPC with the following requirements:

Name of the security group is devops-sg.

The description must be Security group for Nautilus App Servers

Add the inbound rule of type HTTP, with port range of 80. Enter the source CIDR range of 0.0.0.0/0.

Add another inbound rule of type SSH, with port range of 22. Enter the source CIDR range of 0.0.0.0/0.

## Solution

Below are **both AWS Console steps** and **AWS CLI commands** to create the required Security Group.

---

# ✅ **📌 AWS Console Steps (GUI Method)**

### **1. Login to AWS Console**

* Go to **AWS Console**
* Choose region (example: **us-east-1**)

---

### **2. Navigate to Security Groups**

* Search for **VPC**
* Click **Security Groups** (left menu)
* Make sure you are in the **default VPC**

---

### **3. Create Security Group**

Click:

➡️ **Create security group**

Fill the following:

* **Security group name:** `devops-sg`
* **Description:** `Security group for Nautilus App Servers`
* **VPC:** *Select default VPC (vpc-xxxxxx)*

---

### **4. Add Inbound Rules**

Click **Add Rule** (twice)

### **Rule 1 — HTTP**

* **Type:** HTTP
* **Protocol:** TCP
* **Port:** 80
* **Source:** `0.0.0.0/0`

### **Rule 2 — SSH**

* **Type:** SSH
* **Protocol:** TCP
* **Port:** 22
* **Source:** `0.0.0.0/0`

---

### **5. Create the Security Group**

Click **Create security group**

Done! 🎉

---

# ✅ **📌 AWS CLI Steps**

### **1. Get default VPC ID**

```bash
aws ec2 describe-vpcs --filters Name=isDefault,Values=true --query "Vpcs[0].VpcId" --output text
```

Output example:

```
vpc-0abcd1234efgh5678
```

Save it:

```bash
VPC_ID=$(aws ec2 describe-vpcs --filters Name=isDefault,Values=true --query "Vpcs[0].VpcId" --output text)
```

---

### **2. Create the Security Group**

```bash
aws ec2 create-security-group \
  --group-name devops-sg \
  --description "Security group for Nautilus App Servers" \
  --vpc-id $VPC_ID
```

Output:

```
{
    "GroupId": "sg-0123456789abcdef0"
}
```

Save it:

```bash
SG_ID=sg-0123456789abcdef0
```

---

### **3. Add HTTP rule (port 80)**

```bash
aws ec2 authorize-security-group-ingress \
  --group-id $SG_ID \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0
```

---

### **4. Add SSH rule (port 22)**

```bash
aws ec2 authorize-security-group-ingress \
  --group-id $SG_ID \
  --protocol tcp \
  --port 22 \
  --cidr 0.0.0.0/0
```

---

# 🎉 **Security Group Created Successfully**

✔ Name: `devops-sg`
✔ Description: `Security group for Nautilus App Servers`
✔ HTTP (80) from `0.0.0.0/0`
✔ SSH (22) from `0.0.0.0/0`

---

If you want, I can also give you the **Terraform version** or **CloudFormation YAML** for the same!




<img width="1918" height="932" alt="image" src="https://github.com/user-attachments/assets/f651fe8d-f445-42c7-95e9-621d39db988c" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c62e43ec-8769-4fb4-9272-0c54ab13404b" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0acef4a7-f6c6-464d-97d0-789240745ad7" />


