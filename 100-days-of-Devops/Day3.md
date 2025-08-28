Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.
Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter.

## Solution

Got it 👍
The task is to **disable direct SSH login for root** across all application servers.

Here’s the step-by-step:

---

### Steps to Disable Direct Root SSH Login

1. **Login to each App Server**
   Use the jump host and SSH into each app server (e.g., `stapp01`, `stapp02`, `stapp03`).

   ```bash
   ssh tony@stapp01
   ssh steve@stapp02
   ssh banner@stapp03
   ```

2. **Edit the SSH configuration file**
   Open `/etc/ssh/sshd_config` using `vi` or `nano`:

   ```bash
   sudo vi /etc/ssh/sshd_config
   ```

3. **Find the parameter for root login**
   Look for:

   ```
   #PermitRootLogin yes
   ```

   Change it to:

   ```
   PermitRootLogin no
   ```

   (If the line doesn’t exist, add it at the end of the file.)

4. **Restart SSH service**
   After saving the file, restart SSH service:

   ```bash
   sudo systemctl restart sshd
   ```

  

5. **Verify configuration**
   Check that root login is disabled:

   ```bash
   sudo grep PermitRootLogin /etc/ssh/sshd_config
   ```

   Expected output:

   ```
   PermitRootLogin no
   ```

6. **Test**
   From the jump host, try logging in directly as root to confirm it’s blocked:

   ```bash
   ssh root@stapp01
   ```

   It should deny access.

---

✅ Repeat on **all app servers**.
After this, only non-root users can SSH, and root access must be gained via `sudo`.

---

Do you want me to prepare a **one-liner Ansible/Shell script** so you can apply this change to all app servers at once instead of logging in manually?


