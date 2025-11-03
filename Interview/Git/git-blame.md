Good one — this is a **very common interview question**, especially for DevOps or developer-support roles.
adding second line
adding third line
Let’s break it down clearly 👇

---

## 🧩 **What is `git blame` (in simple terms)**

In **layman terms:**

> `git blame` tells you **who changed which line of code and when** in a file.

Think of it like a “history tracker” for each line of a file in Git.

It helps find **who last modified** a specific line — useful when debugging, auditing, or reviewing code changes.

---

## 💡 **Example**

Imagine you have a file called `app.py`:

```python
def add(a, b):
    return a + b
```

Now someone edits it and introduces a bug:

```python
def add(a, b):
    return a - b
```

You see the subtraction bug and want to know *who changed it*.

So, you run:

```bash
git blame app.py
```

You’ll get output like:

```
a3b5e67a (Jim   2024-10-21 12:30:45 +0530 1) def add(a, b):
b4c9f11b (Alice 2024-10-23 15:12:09 +0530 2)     return a - b
```

This shows:

* Line 1 was last modified by **Jim**
* Line 2 (the wrong subtraction) was last modified by **Alice**
* Each line also shows the commit hash and date/time

So now you know **Alice** changed that line — you can check her commit, understand *why* she did it, and fix accordingly.

---

## 🧠 **In Interview Terms (Professional Explanation)**

> The `git blame` command is used to identify **who last modified each line** of a file, **when** it was changed, and **in which commit**.
> It’s especially useful for **troubleshooting**, **code reviews**, and **auditing** to trace the origin of changes in source code.

---

## 🏗️ **Typical Use Cases**

* **Debugging:** Find out who introduced a bug in a line of code.
* **Auditing:** Track who made a sensitive change.
* **Code Review:** Understand the history of a line before modifying it.
* **Knowledge Sharing:** Know who to ask about a specific piece of code.

---

## 🧰 **Useful Variants**

* To view who changed a particular function or block:

  ```bash
  git blame -L 10,20 app.py
  ```

  → shows blame info for lines 10–20 only.

* To ignore whitespace-only changes:

  ```bash
  git blame -w app.py
  ```

---

## 🗣️ **Sample Interview Answer**

> "`git blame` is a Git command that shows which commit and author last modified each line in a file.
> It’s mainly used to trace changes for debugging or auditing. For example, if a bug appears in a function, I can run `git blame` on that file to see who last changed that line and when. This helps me quickly identify the context or person responsible for the modification."

---

Would you like me to also show **how to combine `git blame` with `git show`** to directly see the exact change that person made (very useful in interviews)?
