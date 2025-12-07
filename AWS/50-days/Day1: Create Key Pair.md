The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, create a key pair with the following requirements:

Name of the key pair should be nautilus-kp.

Key pair type must be rsa

## Solution

```
showcreds
aws ec2 describe-key-pairs --query "KeyPairs[*].[KeyName]" --output table
```

<img width="1919" height="344" alt="image" src="https://github.com/user-attachments/assets/25feb66c-8328-43f9-86f6-5c32c902437b" />


-private key will get downloaded automatically, after creation
<img width="1143" height="99" alt="image" src="https://github.com/user-attachments/assets/0fc815ca-eef7-4ad7-9236-7521f32b4642" />


Doing it in the **AWS Console** is very simple. Follow the steps below.

---

# ✅ **Create Key Pair in AWS Console (RSA – nautilus-kp)**

### **Step-1: Login to AWS Console**

Go to:

```
AWS Console → EC2 → Key Pairs
```

You can also search **“Key Pairs”** in the AWS search bar.

---

# **Step-2: Click “Create key pair”**

You will see a button:

👉 **Create key pair**

Click it.

---

# **Step-3: Fill the key pair details**

A form will appear:

### **1️⃣ Key pair name**

```
nautilus-kp
```

### **2️⃣ Key pair type**

Select:

```
RSA
```

### **3️⃣ Private key file format**

Select:

```
. pem   (recommended for Linux / SSH)
```

or

```
. ppk   (if you use PuTTY on Windows)
```

---

# **Step-4: Click “Create key pair”**

After clicking:

✅ AWS will automatically generate the RSA key
✅ Private key file (nautilus-kp.pem) will download automatically
Save it in a safe location.

---

# **Step-5: Verify**

Go to:

```
EC2 → Key Pairs → You should see "nautilus-kp" in the list
```

---

If you want, I can explain how to use the key pair to SSH into an EC2 instance.
