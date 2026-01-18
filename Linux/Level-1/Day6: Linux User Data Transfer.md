Due to an accidental data mix-up, user data was unintentionally mingled on Nautilus App Server 2 at the /home/usersdata location by the Nautilus production support team in Stratos DC. To rectify this, specific user data needs to be filtered and relocated. Here are the details:



Locate all files (excluding directories) owned by user mariyam within the /home/usersdata directory on App Server 2. Copy these files while preserving the directory structure to the /official directory.


## Solution

You can fix this safely using the **`find` command with `-user`, `-type f`, and `--parents`** to preserve directory structure.

Run the following **on App Server 2**:

```bash
sudo find /home/usersdata -type f -user mariyam -exec cp --parents {} /official \;
```

---

### 🔍 Explanation (step by step)

* `find /home/usersdata`
  → Search inside `/home/usersdata`

* `-type f`
  → **Only files** (directories are excluded as required)

* `-user mariyam`
  → Select files **owned by user mariyam**

* `-exec cp --parents {} /official \;`
  → Copy each matched file to `/official`

  * `--parents` **preserves the directory structure**
  * `{}` represents the found file
  * `\;` ends the `-exec` command

---

### 📁 Example

If a file exists at:

```
/home/usersdata/team1/reports/file1.txt
```

It will be copied to:

```
/official/home/usersdata/team1/reports/file1.txt
```

---

### ✅ Verify

```bash
ls -R /official
```

---

### 🧠 One-line summary

👉 This command **copies only mariyam-owned files (not directories) from `/home/usersdata` to `/official` while preserving the original folder structure**.

If you want the **move (cut) version** instead of copy, let me know.
