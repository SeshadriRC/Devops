The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

For this task, create an EC2 instance with following requirements:

1) The name of the instance must be devops-ec2.

2) You can use the Amazon Linux AMI to launch this instance.

3) The Instance type must be t2.micro.

4) Create a new RSA key pair named devops-kp.

5) Attach the default (available by default) security group.

## Solution

Below is a **clean, exam-/lab-friendly AWS CLI solution** to create the EC2 instance exactly as requested.

---

## Step 1: Create RSA key pair `devops-kp`

```bash
aws ec2 create-key-pair \
  --key-name devops-kp \
  --key-type rsa \
  --query 'KeyMaterial' \
  --output text > devops-kp.pem
```

Set correct permissions:

```bash
chmod 400 devops-kp.pem
```

---

## Step 2: Get latest Amazon Linux AMI ID

```bash
aws ec2 describe-images \
  --owners amazon \
  --filters "Name=name,Values=amzn2-ami-hvm-*-x86_64-gp2" \
  --query 'Images | sort_by(@, &CreationDate)[-1].ImageId' \
  --output text
```

➡️ **Copy the AMI ID** from the output (example: `ami-0abcdef12345`)

---

## Step 3: Get default security group ID

```bash
aws ec2 describe-security-groups \
  --filters Name=group-name,Values=default \
  --query 'SecurityGroups[0].GroupId' \
  --output text
```

➡️ **Copy the security group ID**

---

## Step 4: Launch EC2 instance

```bash
aws ec2 run-instances \
  --image-id <AMI_ID> \
  --instance-type t2.micro \
  --key-name devops-kp \
  --security-group-ids <SECURITY_GROUP_ID> \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=devops-ec2}]' \
  --count 1
```

Replace:

* `<AMI_ID>` → Amazon Linux AMI ID
* `<SECURITY_GROUP_ID>` → Default SG ID

---

## Step 5: Verify instance

```bash
aws ec2 describe-instances \
  --filters Name=tag:Name,Values=devops-ec2 \
  --query 'Reservations[*].Instances[*].[InstanceId,State.Name]' \
  --output table
```

---

### ✅ Final Outcome

* EC2 Name: **devops-ec2**
* AMI: **Amazon Linux**
* Instance Type: **t2.micro**
* Key Pair: **devops-kp (RSA)**
* Security Group: **default**

If you want **AWS Console steps** or a **single combined command version**, tell me 👍


***************************************************************************
**Console steps**

Below are **clear, step-by-step AWS Console instructions** to create the EC2 instance exactly as per the requirement.

---

## Step 1: Login to AWS Console

1. Open **[https://console.aws.amazon.com/](https://console.aws.amazon.com/)**
2. Sign in with your AWS credentials
3. Select the **correct region** (top-right corner)

---

## Step 2: Go to EC2 Service

1. In the search bar, type **EC2**
2. Click **EC2** → this opens the EC2 Dashboard
3. Click **Instances** from the left menu
4. Click **Launch instances**

---

## Step 3: Name the EC2 Instance

1. In the **Name and tags** section
2. Enter **Name**:

   ```
   devops-ec2
   ```

---

## Step 4: Choose Amazon Linux AMI

1. Under **Application and OS Images (AMI)**
2. Select **Amazon Linux**

   * (Amazon Linux 2 or Amazon Linux 2023 — both are acceptable unless specified)

---

## Step 5: Choose Instance Type

1. Under **Instance type**
2. Select:

   ```
   t2.micro
   ```

---

## Step 6: Create RSA Key Pair

1. Under **Key pair (login)**
2. Click **Create new key pair**
3. Key pair name:

   ```
   devops-kp
   ```
4. Key pair type:

   ```
   RSA
   ```
5. Private key file format:

   ```
   .pem
   ```
6. Click **Create key pair**

   * The key file will download automatically

---

## Step 7: Select Default Security Group

1. Under **Network settings**
2. Click **Edit**
3. Choose **Select existing security group**
4. Select:

   ```
   default
   ```

   (This is the default security group available by default)

---

## Step 8: Storage (Leave Default)

1. Keep the default root volume settings
2. No changes required

---

## Step 9: Launch Instance

1. Review all settings
2. Click **Launch instance**

---

## Step 10: Verify Instance

1. Click **View all instances**
2. Confirm:

   * Instance name: **devops-ec2**
   * Instance state: **Running**
   * Instance type: **t2.micro**

---

### ✅ Task Completed

Your EC2 instance has been successfully launched with:

* Name: **devops-ec2**
* Amazon Linux AMI
* Instance type: **t2.micro**
* RSA key pair: **devops-kp**
* Default security group

If you want **screenshots guidance**, **troubleshooting**, or **how to SSH into the instance**, just tell me 👍

---

- No key pair. only configure whatever given in question, remaining networking configuration leave it as same

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/61b28c85-ea51-4ca9-a775-e8b719abd9b6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3796a6eb-1a76-4f91-b10c-158322a9a43c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f3ce218b-e7ab-4a0b-97f6-b463b6789547" />
<img width="1919" height="664" alt="image" src="https://github.com/user-attachments/assets/943dc3ad-8eac-4d35-9ff0-6ca705785741" />
<img width="843" height="699" alt="image" src="https://github.com/user-attachments/assets/95701b20-a1ee-4ddc-bfca-3c8ff226b3aa" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/428d3ab3-a082-4351-ace6-049e210e1656" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0c288154-d27f-40d7-94da-d169d2302698" />




