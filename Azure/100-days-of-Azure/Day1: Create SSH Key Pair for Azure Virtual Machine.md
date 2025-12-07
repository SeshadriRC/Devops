The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, create an SSH key pair with the following requirements:

The name of the SSH key pair should be nautilus-kp.

The key pair type must be rsa.

## Solution

```
az sshkey list --query "[].name" --output table

az sshkey create \
  --name nautilus-kp \
  --resource-group <your-rg> \
  --ssh-key-name nautilus-kp

```

**Console method**

ssh keys --> create --> Select resource group --> RSA format -->

<img width="1919" height="547" alt="image" src="https://github.com/user-attachments/assets/7c4b9ab7-fbee-4770-a060-f1e8efb8424e" />

