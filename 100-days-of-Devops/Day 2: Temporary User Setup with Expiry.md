As part of the temporary assignment to the Nautilus project, a developer named ravi requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:

Create a user named ravi on App Server 1 in Stratos Datacenter. Set the expiry date to 2024-04-15, ensuring the user is created in lowercase as per standard protocol.

## Solution

Here’s how you can create the temporary user **ravi** with an expiry date of **2024-04-15** on **App Server 1 in Stratos Datacenter**:

1. First, connect to **App Server 1**:

   ```bash
   ssh banner@<App-Server-1-IP>
   ```

2. Switch to root (if required):

   ```bash
   sudo -i
   ```

3. Create the user `ravi` with expiry date:

   ```bash
   sudo useradd -e 2024-04-15 ravi
   ```

   * `-e` specifies the account expiry date in `YYYY-MM-DD` format.
   * Username must be lowercase (`ravi`).

4. (Optional) Set a password for the user:

   ```bash
   sudo passwd ravi
   ```

5. Verify the expiry date:

   ```bash
   sudo chage -l ravi
   ```

   You should see:

   ```
   Account expires : Apr 15, 2024
   ```

✅ This ensures `ravi` is created as a **temporary user** with automatic expiry.

Do you want me to also add a command to automatically lock and remove the user after expiry, or just rely on expiry disabling the account?
