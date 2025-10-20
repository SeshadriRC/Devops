
Question: How to validate the variable during terraform plan time? Example you take a variable called AMI. We need to make sure its of a proper format.


<img width="1208" height="669" alt="image" src="https://github.com/user-attachments/assets/997b9183-7288-41a8-956a-2a2586de01c2" />



Good question 👏 — You’re referring to **variable validation** in Terraform, which is used to ensure that inputs (like AMI IDs) follow the right pattern *before* applying changes — i.e., at **plan time**.

Let’s fix and explain your example clearly 👇

---

### ✅ **Correct Terraform Variable Example (with validation)**

```hcl
variable "ami" {
  type        = string
  description = "EC2 instance AMI ID"
  default     = "ami-90394321875931"

  validation {
    condition     = length(var.ami) > 4 && substr(var.ami, 0, 4) == "ami-"
    error_message = "Enter a proper AMI ID. It should start with 'ami-' and be longer than 4 characters."
  }
}
```

---

### 🧩 **Explanation**

| Field           | Meaning                                                                                               |
| --------------- | ----------------------------------------------------------------------------------------------------- |
| `validation`    | Used to define checks that must be true for the variable to be valid.                                 |
| `condition`     | The logic to test. If this evaluates to **false**, Terraform throws an error during `terraform plan`. |
| `error_message` | Custom error message shown to the user when validation fails.                                         |

---

### 💡 **Condition Breakdown**

```hcl
length(var.ami) > 4
```

→ Ensures it’s not too short.

```hcl
substr(var.ami, 0, 4) == "ami-"
```

→ Ensures the AMI ID **starts with "ami-"**.

Both conditions must be true (`&&`).

---

### 🧪 **Test Results**

| Input       | Result                 |
| ----------- | ---------------------- |
| `ami-01234` | ✅ Passes               |
| `ami-xyz`   | ✅ Passes               |
| `ame-01234` | ❌ Fails (wrong prefix) |
| `ami-`      | ❌ Fails (too short)    |

---

### 🗣 **Layman summary**

> Terraform checks your variable before deploying — like a spell-check for your inputs.
> Here, it ensures the AMI starts with “ami-” and isn’t too short. If not, Terraform stops at plan time and shows the custom error message.

---

Would you like me to show a **regex-based validation** version too (for more precise AMI format like `ami-[0-9a-f]{8,17}`)?
