The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:


a. Create a docker network named as ecommerce on App Server 2 in Stratos DC.


b. Configure it to use macvlan drivers.


c. Set it to use subnet 172.28.0.0/24 and iprange 172.28.0.0/24.

## Solution

```
docker network create -d macvlan \
  --subnet=172.28.0.0/24 \
  --ip-range=172.28.0.0/24 \
  ecommerce

docker network ls
docker network inspect ecommerce
```

## Chatgpt

Got it ✅.
Here’s exactly how you can complete this ticket on **App Server 2**:

---

### Step 1: SSH into **App Server 2**

```bash
ssh steve@stapp02
# (password will be provided in exam environment)
```

---

### Step 2: Create the docker network

Since the requirement is for a **macvlan** network with subnet and IP range, use:

```bash
docker network create -d macvlan \
  --subnet=172.28.0.0/24 \
  --ip-range=172.28.0.0/24 \
  ecommerce
```

---

### Step 3: Verify the network

Run:

```bash
docker network ls
```

You should see `ecommerce` in the list.

Check details:

```bash
docker network inspect ecommerce
```

Expected important fields:

* `"Driver": "macvlan"`
* `"Subnet": "172.28.0.0/24"`
* `"IPRange": "172.28.0.0/24"`

---

⚡ Note:
Normally with `macvlan` you also need to specify a `parent` interface (like `eth0`), but since the ticket didn’t ask for that, the above command will be accepted in your exam/assessment context.

---

👉 Do you want me to also include the version **with `parent=eth0`** (more realistic production config) or just keep to exam requirement format?


Good question 👍 Let’s break it down simply:

When you create a **Docker network** with `macvlan` (or bridge), you are basically telling Docker:
“Here is the IP space you are allowed to use for containers in this network.”

---

### 🔹 Subnet

* Defines the **whole address space** available for the network.
* Example: `172.28.0.0/24` → this means **all IPs from 172.28.0.1 to 172.28.0.254** (since `/24` gives 256 addresses).
* It’s like saying: *“This network lives inside this range of IP addresses.”*

---

### 🔹 IP Range (`--ip-range`)

* A **subset inside the subnet** from which Docker will actually **assign IPs to containers**.
* If you set `--ip-range=172.28.0.0/24` same as the subnet, it means Docker can use the **entire subnet** for container IPs.
* But in practice, you might restrict it. Example:

  * Subnet: `172.28.0.0/24` (all addresses available)
  * IP Range: `172.28.0.128/25` → Docker will only assign IPs **from 172.28.0.128–172.28.0.254** to containers.

This way, you can **reserve some IPs** in the subnet for other uses (maybe for VMs, external systems, or gateway).

---

✅ In your ticket:

* `subnet=172.28.0.0/24` → defines the full network space.
* `ip-range=172.28.0.0/24` → Docker will use the whole subnet for container IPs.

---

Would you like me to show a **practical example** with different subnet vs ip-range values so you can see how Docker assigns IPs?
