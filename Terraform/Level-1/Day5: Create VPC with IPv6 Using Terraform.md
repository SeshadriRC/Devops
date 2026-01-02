The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the AWS cloud. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Private Clouds (VPCs) as the initial step, as they will be provisioning various services under different VPCs.

For this task, create a VPC named xfusion-vpc in the us-east-1 region with the Amazon-provided IPv6 CIDR block using terraform.

The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.

Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

## Solution

Below is the **correct and minimal Terraform configuration** to create a VPC named **`xfusion-vpc`** in **`us-east-1`** with an **Amazon-provided IPv6 CIDR block**.

👉 As requested, **everything goes into `main.tf` only**.

---

## 📄 `/home/bob/terraform/main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "xfusion_vpc" {
  cidr_block                     = "10.0.0.0/16"
  assign_generated_ipv6_cidr_block = true

  tags = {
    Name = "xfusion-vpc"
  }
}
```

---

## 🔍 Explanation (short)

* `assign_generated_ipv6_cidr_block = true`
  → AWS automatically assigns an **Amazon-provided IPv6 CIDR**
* `cidr_block` is still required for **IPv4**
* VPC will be created in **us-east-1**
* VPC name will be **xfusion-vpc**

---

## ▶️ Commands to run (from `/home/bob/terraform`)

```bash
terraform init
terraform plan
terraform apply -auto-approve
```

---

## ✅ Result

* VPC **xfusion-vpc** created
* IPv4 CIDR: `10.0.0.0/16`
* IPv6 CIDR: **Amazon-provided**
* Region: **us-east-1**

If you want, I can also add:

* Public/private subnets with IPv6
* IGW + route tables
* Terraform output to display IPv6 CIDR

Just tell me 👍
