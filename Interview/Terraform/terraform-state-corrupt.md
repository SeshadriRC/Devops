<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f9ea23a2-273a-497e-bbd5-c9716a700fd3" />

**Question :** When running terraform apply, The process gets hung. Now if you cancel this it might result in corrupt terraform state. How would you debug this issue ?


Below is the **perfect interview answer** — clear, structured, practical, and DevOps-oriented.

---

# ✅ **Interview-Ready Answer: How to debug a hung `terraform apply`**

When `terraform apply` hangs, it usually means Terraform is stuck waiting for a response from the cloud provider (AWS, Azure, GCP) or a long-running API operation.
Cancelling it midway can corrupt the state, so we must debug carefully.

---

# 🔍 **1. Open a second terminal — do NOT kill the first one immediately**

This is the safest method.

In terminal 2, run:

```bash
terraform state pull
```

This helps you check:

* If state is locked
* If the lock is stale
* If the resource creation got partially completed

You’ll see the **current remote state**, even if apply is stuck.

---

# 🔒 **2. Check Terraform state lock**

Most backends (S3 + DynamoDB, Consul, etc.) keep a lock.
Check if the apply is stuck due to a lock problem:

```bash
terraform force-unlock <LOCK_ID>
```

⚠ Only use this if you confirm the original process is dead.

---

# 🧪 **3. Check cloud provider console**

Terraform may be waiting on:

* Stuck AWS resource creation (`RDS`, `EC2`, `LB`, `Subnet`, `IGW`)
* IAM throttling
* Timeout on API calls
* Network issues
* A resource that needs manual approval (like RDS deletion protection)

You must open:

* AWS Console / Azure Portal / GCP Console
* Look at the resource Terraform is trying to create

This usually shows EXACTLY what it’s waiting on.

---

# 📜 **4. Look at Terraform logs (TF_LOG)**

Enable detailed logs:

```bash
export TF_LOG=DEBUG
export TF_LOG_PATH=./tf-debug.log
terraform apply
```

This tells you:

* Which API call Terraform froze on
* Whether provider plugin crashed
* If network/DNS issues are causing the hang

---

# 🔧 **5. Check if provider plugin is stuck**

Occasionally, Terraform hangs because the provider binary freezes.

You can check running processes:

```bash
ps aux | grep terraform
```

Or kill only the **provider plugin**, NOT terraform itself.

---

# 🧼 **6. Run a plan to check drift**

After you stop the hung process safely, run:

```bash
terraform refresh
terraform plan
```

This shows if:

* Resource was created in cloud but not in state
* Resource partially created
* Drift happened

---

# 💣 **7. Manual fix — import or remove resources**

If the resource was created but state is missing:

```bash
terraform import <resource> <id>
```

If resource creation FAILED in cloud:

```bash
terraform state rm <resource>
```

Then re-apply.

---

# 📝 **Strong Interview Summary (say this)**

> “If `terraform apply` hangs, I never kill it directly. I open a second terminal and check the state using `terraform state pull` to avoid corruption. Then I check whether the state is locked, verify the cloud provider console to see what Terraform is waiting on, and enable debug logs using `TF_LOG=DEBUG`.
>
> After resolving the issue, I run `terraform refresh` and `terraform plan` to ensure the state and real resources are in sync. If needed, I import or remove specific resources. This avoids corrupting the state and ensures a clean recovery.”

---

If you want, I can give you a **one-page cheat sheet answer** for quick interview recall.
