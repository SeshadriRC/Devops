The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

For this task, create an EC2 instance using Terraform with the following requirements:

The name of the instance must be nautilus-ec2.

Use the Amazon Linux ami-0c101f26f147fa7fd to launch this instance.

The Instance type must be t2.micro.

Create a new RSA key named nautilus-kp.

Attach the default (available by default) security group.

The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to provision the instance.

Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

## Solution

Below is the **cleanest and simplest** `main.tf` you need for this task.
It uses:

✔ EC2 instance
✔ Amazon Linux AMI
✔ t2.micro
✔ Generates a **new RSA key pair** inside Terraform
✔ Uses the **default security group** automatically created in the default VPC
✔ No additional `.tf` files — only `main.tf`

---

# ✅ **main.tf (Put this inside /home/bob/terraform/main.tf)**

```hcl
provider "aws" {
  region = "us-east-1"
}

# -------------------------------------------------------------------
# 1. Create a new RSA key pair named "nautilus-kp"
# -------------------------------------------------------------------
resource "tls_private_key" "nautilus" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

resource "aws_key_pair" "nautilus_kp" {
  key_name   = "nautilus-kp"
  public_key = tls_private_key.nautilus.public_key_openssh
}

# Save the private key locally (optional but recommended)
resource "local_file" "private_key" {
  content  = tls_private_key.nautilus.private_key_pem
  filename = "${path.module}/nautilus-kp.pem"
}

# -------------------------------------------------------------------
# 2. Fetch the default security group of default VPC
# -------------------------------------------------------------------
data "aws_security_group" "default" {
  name   = "default"
  vpc_id = data.aws_vpc.default.id
}

# Get the default VPC
data "aws_vpc" "default" {
  default = true
}

# -------------------------------------------------------------------
# 3. Create EC2 instance named "nautilus-ec2"
# -------------------------------------------------------------------
resource "aws_instance" "nautilus_ec2" {
  ami                    = "ami-0c101f26f147fa7fd"
  instance_type          = "t2.micro"
  key_name               = aws_key_pair.nautilus_kp.key_name
  vpc_security_group_ids = [data.aws_security_group.default.id]

  tags = {
    Name = "nautilus-ec2"
  }
}
```

---

# ✅ **Commands to run**

Inside `/home/bob/terraform`:

```
terraform init
terraform validate
terraform plan
terraform apply -auto-approve
```

---

# 📌 What this configuration does

### ✔ Creates a 4096-bit RSA key pair

Stored in `nautilus-kp.pem` locally.

### ✔ Uploads the public key to AWS as `nautilus-kp`

### ✔ Reads the **default VPC**

Because AWS always creates a default VPC in every region.

### ✔ Reads the **default security group**

That's the one the question wants you to attach.

### ✔ Launches an EC2 instance:

```
AMI: ami-0c101f26f147fa7fd
Type: t2.micro
Name: nautilus-ec2
```

---

# If you want, I can also provide:

✅ Version with no key creation (use existing key)
✅ Version with user-data script
✅ Version with variables
Just tell me!


`${path.module}` is a **Terraform built-in variable** that returns the **absolute path of the folder where the current .tf file is located**.

---

# 📌 **Meaning of `${path.module}`**

If your Terraform files are inside:

```
/home/bob/terraform
```

Then:

```
${path.module}  ->  /home/bob/terraform
```

So when we write:

```hcl
filename = "${path.module}/nautilus-kp.pem"
```

It becomes:

```
/home/bob/terraform/nautilus-kp.pem
```

---

# 📌 **Why do we use `${path.module}`?**

✔ Works on **any system** (Linux, Windows, Mac)
✔ Always writes inside the directory where Terraform code lives
✔ Does **not** depend on user's current working directory
✔ Makes modules reusable

Example:

| Expression                | Expands To                                     |
| ------------------------- | ---------------------------------------------- |
| `${path.module}/file.txt` | `/home/bob/terraform/file.txt`                 |
| `${path.root}`            | Root of the whole Terraform project            |
| `${path.cwd}`             | Current working directory where command is run |

---

# 📌 Summary

`${path.module}` = "Folder that contains the .tf file".

It helps Terraform write files (like private keys) safely and consistently.

---

If you want, I can show the difference between `path.module`, `path.root`, and `path.cwd` with examples.

