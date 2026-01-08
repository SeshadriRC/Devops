As part of the temporary assignment to the Nautilus project, a developer named rose requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:



Create a user named rose on App Server 1 in Stratos Datacenter. Set the expiry date to 2026-12-07, ensuring the user is created in lowercase as per standard protocol.

Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.

## Solution

This is a **straightforward Linux user management task** on **App Server 1**. Follow these exact steps.

---

## ✅ Requirement Recap

* Server: **App Server 1**
* Username: **rose** (lowercase)
* Expiry date: **2026-12-07**

---

## 1️⃣ SSH into App Server 1

From the jump host (or wherever instructed):

```bash
ssh tony@appserver01
```

*(Username may vary in your lab — use the one shown in “Details of all Users and Servers”)*

---

## 2️⃣ Create the user with expiry date

Run this command as root (or with sudo):

```bash
sudo useradd -e 2026-12-07 rose
```

📌 Explanation:

* `useradd` → create user
* `-e 2026-12-07` → account expiry date
* `rose` → username (lowercase as required)

---

## 3️⃣ (Optional but Recommended) Verify expiry date

```bash
sudo chage -l rose
```

You should see:

```
Account expires : Dec 07, 2026
```

---

## ✅ Final Answer (What the grader expects)

✔ User **rose** created
✔ Expiry date set to **2026-12-07**
✔ User created in lowercase
✔ Done on **App Server 1**

---

## ⚠️ Common Mistakes to Avoid

* ❌ Using uppercase (`Rose`)
* ❌ Using wrong date format
* ❌ Creating user on wrong server
* ❌ Forgetting `-e` option

---

If you want, I can also explain:

* Difference between **password expiry** vs **account expiry**
* How to set expiry using `chage`
* How graders usually validate this task

Just tell me 👍
