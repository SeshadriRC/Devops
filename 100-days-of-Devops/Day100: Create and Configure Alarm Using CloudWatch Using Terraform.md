The Nautilus DevOps team has been tasked with setting up an EC2 instance for their application. To ensure the application performs optimally, they also need to create a CloudWatch alarm to monitor the instance's CPU utilization. The alarm should trigger if the CPU utilization exceeds 90% for one consecutive 5-minute period. To send notifications, use the SNS topic named datacenter-sns-topic, which is already created.

Launch EC2 Instance: Create an EC2 instance named datacenter-ec2 using any appropriate Ubuntu AMI (you can use AMI ami-0c02fb55956c7d316).

Create CloudWatch Alarm: Create a CloudWatch alarm named datacenter-alarm with the following specifications:

Statistic: Average
Metric: CPU Utilization
Threshold: >= 90% for 1 consecutive 5-minute period
Alarm Actions: Send a notification to the datacenter-sns-topic SNS topic.
Update the main.tf file (do not create a separate .tf file) to create a EC2 Instance and CloudWatch Alarm.

Create an outputs.tf file to output the following values:

KKE_instance_name for the EC2 instance name.
KKE_alarm_name for the CloudWatch alarm name.

Notes:

The Terraform working directory is /home/bob/terraform.

Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

Before submitting the task, ensure that terraform plan returns No changes. Your infrastructure matches the configuration.


# Solution

Below is a **correct, ready-to-apply Terraform configuration** that meets your exact task requirements.

You only need:

* **main.tf** (contains EC2 + CloudWatch Alarm)
* **outputs.tf**

Everything is simple and minimal—exactly as required.

---

# ✅ **1. main.tf**

```hcl
# ----------------------------
# EC2 Instance
# ----------------------------
resource "aws_instance" "datacenter_ec2" {
  ami           = "ami-0c02fb55956c7d316" # Ubuntu AMI
  instance_type = "t2.micro"

  tags = {
    Name = "datacenter-ec2"
  }
}

# ----------------------------
# CloudWatch Alarm
# ----------------------------
resource "aws_cloudwatch_metric_alarm" "datacenter_alarm" {
  alarm_name          = "datacenter-alarm"
  comparison_operator = "GreaterThanOrEqualToThreshold"
  evaluation_periods  = 1
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300                # 5 minutes
  statistic           = "Average"
  threshold           = 90
  alarm_description   = "Triggers when EC2 CPU Utilization >= 90%"
  alarm_actions       = ["arn:aws:sns:us-east-1:000000000000:datacenter-sns-topic"]

  dimensions = {
    InstanceId = aws_instance.datacenter_ec2.id
  }
}
```

---

# ⚠️ **IMPORTANT**

Replace the SNS ARN below with the correct AWS account ID:

```
arn:aws:sns:us-east-1:<YOUR-ACCOUNT-ID>:datacenter-sns-topic
```

---

# ✅ **2. outputs.tf**

```hcl
output "KKE_instance_name" {
  value = aws_instance.datacenter_ec2.tags["Name"]
}

output "KKE_alarm_name" {
  value = aws_cloudwatch_metric_alarm.datacenter_alarm.alarm_name
}
```

---

# 🚀 **Steps to Run**

```bash
cd /home/bob/terraform
terraform init
terraform validate
terraform apply -auto-approve
```

Then verify:

```bash
terraform plan
```

You should see:

```
No changes. Your infrastructure matches the configuration.
```

---

If you want, I can also:

✅ Add variables
✅ Add security group
✅ Add key pair
✅ Add user data
Just tell me!
