Got it 👍

Since you need to create a user **mark** with a **non-interactive shell**, the usual approach is to assign a shell like `/sbin/nologin` or `/bin/false`. This prevents the user from logging in interactively, but still allows processes/services to run under that user if needed.

Here’s the exact command you can run on **App Server 3**:

```bash
sudo useradd -s /sbin/nologin mark
```

✅ Explanation:

* `useradd` → creates the user
* `-s /sbin/nologin` → sets the login shell to **nologin**, making it non-interactive
* `mark` → username

If `/sbin/nologin` is not available on your system, you can use `/bin/false`:

```bash
sudo useradd -s /bin/false mark
```

---

👉 To verify:

```bash
getent passwd mark
```

Output should look like:

```
mark:x:1002:1002::/home/mark:/sbin/nologin
```

---

Do you also want me to include **creating the home directory** for `mark`, or should it be a pure service user (no home directory)?
