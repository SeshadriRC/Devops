The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, allocate an Elastic IP address named datacenter-eip using Terraform.

The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.

Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

## Solution

You need to create a **Terraform configuration** that allocates an **Elastic IP** with the name **datacenter-eip**.

Below is exactly what you should put in **`/home/bob/terraform/main.tf`**.

---

### `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_eip" "datacenter_eip" {
  tags = {
    Name = "datacenter-eip"
  }
}
```

---

### Steps to execute (after creating `main.tf`)

From the **integrated terminal**:

```bash
cd /home/bob/terraform
terraform init
terraform plan
terraform apply -auto-approve
```

---

### What this does (quick explanation)

* **provider "aws"** → tells Terraform to use AWS (default region set)
* **aws_eip** → allocates an Elastic IP in AWS
* **tags.Name** → names the EIP as `datacenter-eip` (important for verification)

This follows the requirement:
✔ Uses Terraform
✔ Creates only `main.tf`
✔ Allocates an Elastic IP with the correct name

If you want, I can also show how to **verify the EIP from AWS CLI** or **destroy it after validation**.


```
aws ec2 describe-addresses --output table
```
