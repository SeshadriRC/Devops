The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, create a key pair using Terraform with the following requirements:

Name of the key pair should be devops-kp.

Key pair type must be rsa.

The private key file should be saved under /home/bob/devops-kp.pem.
The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.

## Answer

Alright 🚀 Let’s build this step by step. You need a **Terraform config** (`main.tf`) to create an **AWS key pair** named `devops-kp`, with **RSA type**, and store the **private key** at `/home/bob/devops-kp.pem`.

Here’s what your `main.tf` (inside `/home/bob/terraform/`) should look like:

```hcl

# Generate RSA private key
resource "tls_private_key" "devops_key" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

# Create AWS key pair using the public key
resource "aws_key_pair" "devops_kp" {
  key_name   = "devops-kp"
  public_key = tls_private_key.devops_key.public_key_openssh
}

# Save the private key to a local file
resource "local_file" "private_key" {
  content  = tls_private_key.devops_key.private_key_pem
  filename = "/home/bob/devops-kp.pem"
}
```

---

### Steps to run:

1. Go to the working directory:

   ```bash
   cd /home/bob/terraform
   ```

2. Initialize Terraform:

   ```bash
   terraform init
   ```

3. Plan the execution:

   ```bash
   terraform plan
   ```

4. Apply the config:

   ```bash
   terraform apply -auto-approve
   ```

---

✅ After this:

* An AWS key pair named **`devops-kp`** will be created.
* The private key will be saved at **`/home/bob/devops-kp.pem`**.
* You can then use it for SSH access to EC2 instances.


---

Do you want me to also add an **output block** in `main.tf` so that Terraform prints the private key location (or public key) after apply?
