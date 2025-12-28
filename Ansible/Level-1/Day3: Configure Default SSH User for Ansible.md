The Nautilus DevOps team aims to manage all servers within the stack using Ansible, utilizing a common sudo user across all servers. They plan to use this user for various tasks on each server. While this isn't finalized, they're starting with testing. Ansible is already installed on the jump host via yum. Here's the requirement:


On the jump host, modify the default configuration of Ansible to enable the use of kirsty as the default SSH user for all hosts. Ensure to make changes within Ansible's default configuration without creating a new one.

## Solution

You need to update **Ansible’s default configuration file on the jump host** so that it uses **`kirsty` as the default SSH user** for all managed hosts.

Since the requirement clearly says **do not create a new config**, we must modify the existing `/etc/ansible/ansible.cfg`.

```
thor@jumphost ~$ ansible --version
ansible [core 2.14.17]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/thor/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.9/site-packages/ansible
  ansible collection location = /home/thor/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.9.18 (main, Jan 24 2024, 00:00:00) [GCC 11.4.1 20231218 (Red Hat 11.4.1-3)] (/usr/bin/python3)
  jinja version = 3.1.2
  libyaml = True
thor@jumphost ~$ cat /etc/ansible/ansible.cfg
# Since Ansible 2.12 (core):
# To generate an example config file (a "disabled" one with all default settings, commented out):
#               $ ansible-config init --disabled > ansible.cfg
#
# Also you can now have a more complete file by including existing plugins:
# ansible-config init --disabled -t all > ansible.cfg

# For previous versions of Ansible you can check for examples in the 'stable' branches of each version
# Note that this file was always incomplete  and lagging changes to configuration settings

# for example, for 2.9: https://github.com/ansible/ansible/blob/stable-2.9/examples/ansible.cfg
[defaults]
host_key_checking = False
remote_user=kirsty
```

---

### ✅ Steps to configure `kirsty` as the default Ansible SSH user

#### 1. Open the default Ansible configuration file

```bash
sudo vi /etc/ansible/ansible.cfg
```

---

#### 2. Locate the `[defaults]` section

If it already exists, modify it.
If it exists but commented, uncomment it.

Add or update this line:

```ini
[defaults]
remote_user = kirsty
```

⚠️ If `remote_user` exists but is commented (`#remote_user`), just remove the `#`.

---

#### 3. Save and exit

```bash
:wq
```

---

### ✅ Verification (Optional but recommended)

Run:

```bash
ansible --version
```

You should see:

```
config file = /etc/ansible/ansible.cfg
```

Then test:

```bash
ansible all -m ping
```

Ansible will now attempt to connect as **`kirsty` by default**.

---

### 🔍 Important Notes (Interview / Practical Insight)

* `remote_user` in `ansible.cfg` applies **globally** to all inventories.
* This can still be overridden by:

  * Inventory (`ansible_user`)
  * Playbook (`remote_user`)
  * CLI (`-u user`)
* Since they plan a **common sudo user**, this approach aligns with best practices for centralized automation.

If you want, I can also explain **how sudo access should be configured for `kirsty`** to avoid password prompts.



