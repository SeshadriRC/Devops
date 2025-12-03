The DevOps team has been tasked with creating a secure DynamoDB table and enforcing fine-grained access control using IAM. This setup will allow secure and restricted access to the table from trusted AWS services only.

As a member of the Nautilus DevOps Team, your task is to perform the following using Terraform:

Create a DynamoDB Table: Create a table named nautilus-table with minimal configuration.

Create an IAM Role: Create an IAM role named nautilus-role that will be allowed to access the table.

Create an IAM Policy: Create a policy named nautilus-readonly-policy that should grant read-only access (GetItem, Scan, Query) to the specific DynamoDB table and attach it to the role.

Create the main.tf file (do not create a separate .tf file) to provision the table, role, and policy.

Create the variables.tf file with the following variables:

KKE_TABLE_NAME: name of the DynamoDB table
KKE_ROLE_NAME: name of the IAM role
KKE_POLICY_NAME: name of the IAM policy
Create the outputs.tf file with the following outputs:

kke_dynamodb_table: name of the DynamoDB table
kke_iam_role_name: name of the IAM role
kke_iam_policy_name: name of the IAM policy
Define the actual values for these variables in the terraform.tfvars file.

Ensure that the IAM policy allows only read access and restricts it to the specific DynamoDB table created.


Notes:

The Terraform working directory is /home/bob/terraform.

Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

Before submitting the task, ensure that terraform plan returns No changes. Your infrastructure matches the configuration.

## Solution

Below is a **complete, correct, ready-to-run Terraform configuration** for your task.

You will create **4 files**:

* `main.tf`
* `variables.tf`
* `outputs.tf`
* `terraform.tfvars`

The configuration meets all requirements:

✔ Creates DynamoDB table
✔ Creates IAM Role
✔ Creates Read-only IAM Policy for that table
✔ Attaches policy → role
✔ Uses variables
✔ Uses outputs
✔ `terraform plan` → No changes (after apply)

---

# ✅ **1. main.tf**

```hcl
# -------------------------------
# DynamoDB Table
# -------------------------------
resource "aws_dynamodb_table" "nautilus_table" {
  name         = var.KKE_TABLE_NAME
  billing_mode = "PAY_PER_REQUEST"

  hash_key = "id"

  attribute {
    name = "id"
    type = "S"
  }
}

# -------------------------------
# IAM Role
# -------------------------------
resource "aws_iam_role" "nautilus_role" {
  name = var.KKE_ROLE_NAME

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Principal = {
        Service = "ec2.amazonaws.com"   # or ec2.amazonaws.com / any trusted service
      }
      Action = "sts:AssumeRole"
    }]
  })
}

# -------------------------------
# IAM Policy (Read-only Access)
# -------------------------------
resource "aws_iam_policy" "nautilus_policy" {
  name        = var.KKE_POLICY_NAME
  description = "Read-only access to specific DynamoDB table"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect   = "Allow"
      Action   = [
        "dynamodb:GetItem",
        "dynamodb:Scan",
        "dynamodb:Query"
      ]
      Resource = aws_dynamodb_table.nautilus_table.arn
    }]
  })
}

# -------------------------------
# Attach Policy to Role
# -------------------------------
resource "aws_iam_role_policy_attachment" "attach" {
  role       = aws_iam_role.nautilus_role.name
  policy_arn = aws_iam_policy.nautilus_policy.arn
}
```

---

# ✅ **2. variables.tf**

```hcl
variable "KKE_TABLE_NAME" {
  description = "Name of DynamoDB table"
  type        = string
}

variable "KKE_ROLE_NAME" {
  description = "Name of IAM Role"
  type        = string
}

variable "KKE_POLICY_NAME" {
  description = "Name of IAM Policy"
  type        = string
}
```

---

# ✅ **3. outputs.tf**

```hcl
output "kke_dynamodb_table" {
  value = aws_dynamodb_table.nautilus_table.name
}

output "kke_iam_role_name" {
  value = aws_iam_role.nautilus_role.name
}

output "kke_iam_policy_name" {
  value = aws_iam_policy.nautilus_policy.name
}
```

---

# ✅ **4. terraform.tfvars**

```hcl
KKE_TABLE_NAME  = "nautilus-table"
KKE_ROLE_NAME   = "nautilus-role"
KKE_POLICY_NAME = "nautilus-readonly-policy"
```

---

# 🚀 **How to run**

```bash
cd /home/bob/terraform
terraform init
terraform validate
terraform apply -auto-approve
```

After applying:

```bash
terraform plan
```

You must see:

```
No changes. Your infrastructure matches the configuration.
```

---

If you want, I can also help you:

✅ Add tags
✅ Add write access
✅ Add resource-based policy
✅ Use modules
✅ Convert to Terragrunt

Just tell me!


```
