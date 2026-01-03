The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

Create a volume with the following requirements:

Name of the volume should be nautilus-volume.

Volume type must be gp3.

Volume size must be 2 GiB.

# Solution

Here’s how you can **create the required EBS volume in AWS**.

---

## Option 1: Using AWS Console (Easy & Common)

1. Log in to **AWS Management Console**
2. Go to **EC2**
3. In the left menu, click **Volumes** (under *Elastic Block Store*)
4. Click **Create volume**
5. Set the following values:

   * **Volume name**: `nautilus-volume`
   * **Volume type**: `gp3`
   * **Size**: `2 GiB`
   * **Availability Zone**:
     ⚠️ Must match the AZ of the EC2 instance you plan to attach it to (e.g., `us-east-1a`)
6. Leave other settings as default
7. Click **Create volume**

✅ Volume will be created successfully.

---

## Option 2: Using AWS CLI

```bash
aws ec2 create-volume \
  --availability-zone us-east-1a \
  --size 2 \
  --volume-type gp3 \
  --tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=nautilus-volume}]'
```

📌 Replace `us-east-1a` with the correct Availability Zone if needed.

---

### Quick Verification

```bash
aws ec2 describe-volumes --filters Name=tag:Name,Values=nautilus-volume

aws ec2 describe-volumes \
  --filters Name=tag:Name,Values=nautilus-volume \
  --query 'Volumes[*].[VolumeId,VolumeType,Size,AvailabilityZone,State]' \
  --output table
```

---

If you want, I can also guide you on **attaching this volume to an EC2 instance** or **formatting & mounting it on Linux**.

<img width="1919" height="803" alt="image" src="https://github.com/user-attachments/assets/699bda00-3cfe-4ee6-bb06-eb8cd2d7545d" />
<img width="1919" height="824" alt="image" src="https://github.com/user-attachments/assets/4912ef14-d5d6-4e16-9b91-cbb5cc544521" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/45777f2b-0beb-42e0-990d-b4c3960dc17c" />
<img width="1919" height="779" alt="image" src="https://github.com/user-attachments/assets/b741955b-9b24-4df4-a5c2-211140f25f84" />




