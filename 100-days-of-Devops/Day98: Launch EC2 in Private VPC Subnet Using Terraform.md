The Nautilus DevOps team is expanding their AWS infrastructure and requires the setup of a private Virtual Private Cloud (VPC) along with a subnet. This VPC and subnet configuration will ensure that resources deployed within them remain isolated from external networks and can only communicate within the VPC. Additionally, the team needs to provision an EC2 instance under the newly created private VPC. This instance should be accessible only from within the VPC, allowing for secure communication and resource management within the AWS environment.

Create a VPC named datacenter-priv-vpc with the CIDR block 10.0.0.0/16.

Create a subnet named datacenter-priv-subnet inside the VPC with the CIDR block 10.0.1.0/24 and auto-assign IP option must not be enabled.

Create an EC2 instance named datacenter-priv-ec2 inside the subnet and instance type must be t2.micro.

Ensure the security group of the EC2 instance allows access only from within the VPC's CIDR block.

Create the main.tf file (do not create a separate .tf file) to provision the VPC, subnet and EC2 instance.

Use variables.tf file with the following variable names:

KKE_VPC_CIDR for the VPC CIDR block.
KKE_SUBNET_CIDR for the subnet CIDR block.
Use the outputs.tf file with the following variable names:

KKE_vpc_name for the name of the VPC.
KKE_subnet_name for the name of the subnet.
KKE_ec2_private for the name of the EC2 instance.

Notes:

The Terraform working directory is /home/bob/terraform.

Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

Before submitting the task, ensure that terraform plan returns No changes. Your infrastructure matches the configuration.


# Solution

Below is the **complete Terraform solution** exactly as required:

* **All resources in main.tf**
* **variables.tf** only declaring the two variables
* **outputs.tf** with the required output variable names
* Works in **/home/bob/terraform**
* Meets **all constraints** (private VPC, subnet, SG allowing internal-only access, EC2 instance in subnet)

---

# ✅ **main.tf**

```hcl
provider "aws" {
  region = "us-east-1"
}

# -------------------------
# VPC
# -------------------------
resource "aws_vpc" "priv_vpc" {
  cidr_block           = var.KKE_VPC_CIDR
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "datacenter-priv-vpc"
  }
}

# -------------------------
# Subnet
# -------------------------
resource "aws_subnet" "priv_subnet" {
  vpc_id                  = aws_vpc.priv_vpc.id
  cidr_block              = var.KKE_SUBNET_CIDR
  map_public_ip_on_launch = false   # Auto-assign IP must not be enabled

  tags = {
    Name = "datacenter-priv-subnet"
  }
}

# -------------------------
# Security Group
# Allow traffic ONLY from inside the VPC CIDR
# -------------------------
resource "aws_security_group" "priv_sg" {
  name        = "datacenter-priv-sg"
  description = "Allow traffic only from within VPC"
  vpc_id      = aws_vpc.priv_vpc.id

  ingress {
    description = "Allow VPC internal traffic"
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = [var.KKE_VPC_CIDR]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# -------------------------
# EC2 Instance
# -------------------------
resource "aws_instance" "priv_ec2" {
  ami           = "ami-0c02fb55956c7d316"   # Amazon Linux 2 (region us-east-1)
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.priv_subnet.id
  vpc_security_group_ids = [aws_security_group.priv_sg.id]

  tags = {
    Name = "datacenter-priv-ec2"
  }
}
```

---

# ✅ **variables.tf**

```hcl
variable "KKE_VPC_CIDR" {
  type        = string
  description = "CIDR block for the VPC"
}

variable "KKE_SUBNET_CIDR" {
  type        = string
  description = "CIDR block for the subnet"
}
```

---

# ✅ **outputs.tf**

```hcl
output "KKE_vpc_name" {
  value = aws_vpc.priv_vpc.tags["Name"]
}

output "KKE_subnet_name" {
  value = aws_subnet.priv_subnet.tags["Name"]
}

output "KKE_ec2_private" {
  value = aws_instance.priv_ec2.tags["Name"]
}
```

---

# ✅ **Example terraform.tfvars**

(You will use this file in your working directory)

```hcl
KKE_VPC_CIDR   = "10.0.0.0/16"
KKE_SUBNET_CIDR = "10.0.1.0/24"
```

---

# ▶️ **How to Run**

Inside **/home/bob/terraform**:

```
terraform init
terraform validate
terraform plan
terraform apply -auto-approve
terraform plan   # must show "No changes." before submission
```

---

# ✔️ This configuration meets all requirements

### ✓ VPC name: **datacenter-priv-vpc**

### ✓ Subnet name: **datacenter-priv-subnet**

### ✓ EC2 name: **datacenter-priv-ec2**

### ✓ Private VPC + private subnet

### ✓ EC2 accessible only inside VPC CIDR

### ✓ Auto-assign IP is disabled

### ✓ Uses variables + outputs exactly as stated

---

If you want, I can also generate:

✅ Architecture diagram
✅ README.md
✅ Comments added version for learning

Would you like those?
