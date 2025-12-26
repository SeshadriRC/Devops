The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the AWS cloud. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Private Clouds (VPCs) as the initial step, as they will be provisioning various services under different VPCs.

Create a VPC named datacenter-vpc in us-east-1 region with 192.168.0.0/24 IPv4 CIDR using terraform.


The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.

Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.



## Solution

Below is the **complete `main.tf`** content you need to create the VPC using Terraform, exactly as requested.

### Steps

1. Open **Integrated Terminal** in VS Code.
2. Navigate to the Terraform directory:

   ```bash
   cd /home/bob/terraform
   ```
3. Create and edit `main.tf`:

   ```bash
   vi main.tf
   ```

---

### `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "datacenter_vpc" {
  cidr_block           = "192.168.0.0/24"
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "datacenter-vpc"
  }
}
```

---

### Apply the configuration

```bash
terraform init
terraform plan
terraform apply
```

Type **yes** when prompted.

✅ This will create a VPC named **datacenter-vpc** in **us-east-1** with CIDR **192.168.0.0/24**.

If you want, I can also help you add **subnets, IGW, route tables**, or validate the VPC using AWS CLI.

In an AWS VPC, these two settings control **DNS behavior** inside the VPC. They are very important for how instances and AWS services communicate.

---

## 1️⃣ `enable_dns_support = true`

### What it does

* Enables **Amazon-provided DNS resolution** in the VPC.
* Instances in the VPC can resolve:

  * Public domain names (e.g., `google.com`)
  * AWS service endpoints (e.g., `s3.amazonaws.com`)
  * Private DNS names inside the VPC

### If set to `false`

* EC2 instances **cannot resolve DNS names**
* You would need to configure **your own DNS server**
* Most AWS services will **not work properly**

### Practical impact

✔ Required for:

* Internet access by name
* AWS APIs
* Load balancers
* RDS endpoints
* EKS / OpenShift / Kubernetes integrations

---

## 2️⃣ `enable_dns_hostnames = true`

### What it does

* Assigns **DNS hostnames** to resources in the VPC.
* EC2 instances get:

  * Public DNS name (if public IP exists)
  * Private DNS name (always)

Example:

```
ip-192-168-0-10.ec2.internal
```

### If set to `false`

* Instances **will not get DNS hostnames**
* Only IP-based communication works

### Practical impact

✔ Required for:

* ALB / NLB / ELB
* RDS connections using endpoint names
* EKS worker nodes
* Auto Scaling groups
* Service discovery

---

## ⚠️ Important Relationship Between Both

| Setting                | Must be true?                     | Why                     |
| ---------------------- | --------------------------------- | ----------------------- |
| `enable_dns_support`   | Yes                               | Core DNS resolution     |
| `enable_dns_hostnames` | Only works if DNS support is true | Hostnames depend on DNS |

👉 **If `enable_dns_support = false`, hostnames won't work even if enabled**

---

## Real-world recommendation (Interview-ready)

> Always enable both unless you have a **very specific reason not to**. Almost all AWS-managed services assume these are enabled.

---

## Quick analogy

* `enable_dns_support` → **DNS server exists**
* `enable_dns_hostnames` → **Resources are given names**

---

If you want, I can also explain:

* Difference between **public vs private DNS**
* Impact on **RDS / ELB / EKS**
* What happens in **custom DNS setups**

Just tell me 👍

