# **Checking and Handling Oversized Files in a Git Repository**

Oversized files can **bloat your Git repository** and make operations like cloning, fetching, and pushing painfully slow.
This guide explains how to **detect**, **trace**, **remove**, and **prevent** large files in Git.

---

## **1. Check for Oversized Files Before `git add`**

To avoid accidentally staging large files, scan your working directory **before running `git add`**.

### **Find all files larger than 50 MB**

```bash
find . -type f -size +50M -not -path "./.git/*"
```

* `+50M` → finds files larger than **50 MB**.
* Excludes the `.git` folder.
* Adjust `50M` to your preferred limit.

---

## **2. Find Large Files Already Tracked by Git**

If files are **already staged or committed**, they’re stored as compressed blobs in `.git/objects/`.

### **List largest Git objects**

```bash
git verify-pack -v .git/objects/pack/*.idx \
  | sort -k3 -n \
  | tail -10
```

* Shows the **10 largest objects** in the repository.
* Output format:

  ```
  <object-hash> <type> <size> <other-info>
  ```

### **Map object hashes to filenames**

```bash
git rev-list --objects --all | grep <object-hash>
```

Example:

```bash
git rev-list --objects --all | grep 38329266
```

* This shows **which file** corresponds to that large object.

---

## **3. Check Staged but Uncommitted Large Files**

To scan files already staged for commit:

```bash
git diff --cached --name-only | while read f; do
    if [ -f "$f" ]; then
        size=$(du -m "$f" | cut -f1)
        if [ "$size" -gt 50 ]; then
            echo " $f is ${size}MB"
        fi
    fi
done
```

* Checks only staged files.
* Replace `50` with your size threshold.

---

## **4. Remove Oversized Files From Git History**

If you find that large files were committed in the past, you can remove them **safely** using [`git-filter-repo`](https://github.com/newren/git-filter-repo):

```bash
git filter-repo --strip-blobs-bigger-than 50M
```

* Rewrites the repository history.
* Completely removes all blobs larger than **50 MB**.
* You need to force-push afterward:

```bash
git push origin --force
```

> **⚠ Warning:** Rewriting history affects everyone working on the repo. Coordinate with your team before doing this.

---

## **5. Prevent Oversized Files in the Future**

Set up a **pre-commit hook** to block files larger than a specific size.

### **Create `.git/hooks/pre-commit`**

```bash
#!/bin/bash
max_size=50  # MB

files=$(git diff --cached --name-only)
for file in $files; do
    if [ -f "$file" ]; then
        size=$(du -m "$file" | cut -f1)
        if [ "$size" -gt $max_size ]; then
            echo " Error: $file is ${size}MB — exceeds ${max_size}MB limit."
            exit 1
        fi
    fi
done
```

Make it executable:

```bash
chmod +x .git/hooks/pre-commit
```

---

## **6. Quick All-in-One Command**

If you want a **single command** to list **both tracked and untracked** files larger than 50 MB:

```bash
find . -type f -size +50M -not -path "./.git/*" -exec du -h {} \;
```

---

## **Summary Table**

| **Task**                        | **Command**                                                             |
| ------------------------------- | ----------------------------------------------------------------------- |
| Find large files (working dir)  | `find . -type f -size +50M -not -path "./.git/*"`                       |
| Find largest Git objects        | `git verify-pack -v .git/objects/pack/*.idx \| sort -k3 -n \| tail -10` |
| Map object hash → filename      | `git rev-list --objects --all \| grep <hash>`                           |
| Check staged large files        | *(see staged scan script above)*                                        |
| Remove large files from history | `git filter-repo --strip-blobs-bigger-than 50M`                         |
| Prevent committing large files  | Use **pre-commit hook**                                                 |

---

## **7. Tips**

* Always **scan before adding** large datasets or binaries.
* Prefer using **Git LFS** for files > 50 MB.
* Regularly check your repo size:

  ```bash
  du -sh .git
  ```

---

## **References**

* [Git Large File Storage (LFS)](https://git-lfs.github.com/)
* [git-filter-repo documentation](https://github.com/newren/git-filter-repo)

