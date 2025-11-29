The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

Use Terraform to create a security group under the default VPC with the following requirements:

1) The name of the security group must be xfusion-sg.

2) The description must be Security group for Nautilus App Servers.

3) Add an inbound rule of type HTTP, with a port range of 80, and source CIDR range 0.0.0.0/0.

4) Add another inbound rule of type SSH, with a port range of 22, and source CIDR range 0.0.0.0/0.

Ensure that the security group is created in the us-east-1 region using Terraform. The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.

Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

## Solution


Below is the **exact Terraform configuration** you should place in **main.tf** to create the security group under the **default VPC** in **us-east-1**.

Since you already have **provider.tf**, we will ONLY write the security group resource inside **main.tf**.

---

# ✅ **main.tf (Place this in /home/bob/terraform/main.tf)**

```hcl
# Get default VPC
data "aws_vpc" "default" {
  default = true
}

# Create Security Group
resource "aws_security_group" "xfusion_sg" {
  name        = "xfusion-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id

  # Inbound rule - HTTP (port 80)
  ingress {
    description = "Allow HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Inbound rule - SSH (port 22)
  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Outbound rule (allow all)
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "xfusion-sg"
  }
}
```

---

# ✅ **Commands to run**

Assuming you are already in **/home/bob/terraform**:

```
terraform init
terraform validate
terraform plan
terraform apply
```

Type **yes** when asked.

---

# 💡 **How to verify creation**

You can check via:

### Terraform:

```
terraform show
```

Or filter only security groups:

```
terraform state list | grep security_group
```

### AWS CLI (if available):

```
aws ec2 describe-security-groups --region us-east-1 --group-names xfusion-sg
```

---

If you want, I can also help you test or explain each block line-by-line.


Here is a **clear and simple explanation** for each part of your Terraform code.

---

# ✅ **1. What is this block?**

```hcl
data "aws_vpc" "default" {
  default = true
}
```

### **What does it mean?**

* This is a **data source** in Terraform.
* A *data source* is used to **read an existing resource** instead of creating a new one.

### **Why do we need this?**

AWS accounts *already* have a **default VPC** created automatically.
Since your requirement is **“create the security group under the default VPC”**, you must fetch the default VPC ID.

Terraform cannot assume which VPC is default — so you tell it:

* `default = true` → *Get the AWS default VPC*

### ✔️ Output of this data source:

This gives us:

* `data.aws_vpc.default.id` → The VPC ID (e.g., vpc-123456789)

---

# ✅ **2. Why we use `vpc_id = data.aws_vpc.default.id`**

```hcl
vpc_id = data.aws_vpc.default.id
```

### **Purpose:**

Every security group **must be attached to a VPC**.

You want your SG to live inside the **default VPC**, not inside the custom VPC you created earlier.

So we assign:

* `vpc_id =` **the ID of the default VPC**
* And we get that VPC ID from the **data source** we just queried.

### ✔️ This ensures the SG is created in:

```
us-east-1 → default VPC
```

---

# ✅ **3. Why protocol is `"tcp"` for ingress**

```hcl
ingress {
  from_port = 80
  to_port   = 80
  protocol  = "tcp"
}
```

### **Why tcp?**

* HTTP (port 80) and SSH (port 22) **both run on TCP**.
* So we specify TCP explicitly.

Examples:

| Service | Port | Protocol |
| ------- | ---- | -------- |
| HTTP    | 80   | TCP      |
| HTTPS   | 443  | TCP      |
| SSH     | 22   | TCP      |
| RDP     | 3389 | TCP      |

So using `"tcp"` here is correct.

---

# ✅ **4. Why protocol is `"-1"` for egress**

```hcl
egress {
  from_port = 0
  to_port   = 0
  protocol  = "-1"
}
```

### **Meaning of protocol = "-1"**

* `-1` means **all protocols**.
* AWS uses `-1` to represent **ANY traffic**.

### **Why do we use it?**

Because **default outbound rule** for a security group allows **all traffic** (HTTP, HTTPS, DNS, ICMP, SSH, NTP… everything).

So:

✔️ `protocol = -1` = allow all
✔️ `from_port = 0`, `to_port = 0` = port range ignored (since protocol = ALL)

---

# 🔥 Quick Summary

| Code                               | Explanation                      |
| ---------------------------------- | -------------------------------- |
| `data "aws_vpc" "default"`         | Reads the *existing* default VPC |
| `vpc_id = data.aws_vpc.default.id` | Places SG inside that VPC        |
| `protocol = "tcp"`                 | HTTP (80) & SSH (22) use TCP     |
| `protocol = "-1"`                  | Allow all outbound traffic       |

---

If you want, I can also explain “what is a security group internally”, or help you create SG modules, subnets, or EC2 next.

