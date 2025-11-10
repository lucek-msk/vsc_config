# 🚀 SSH Setup for GitHub on Windows — Step-by-Step Guide

> **Goal:** Generate an SSH key, add it to GitHub, test the connection, and push your code via SSH.  
> **Prerequisites:** Git is installed and you have a GitHub account.

---

## 1️⃣ Verify Git and Configure User Info
Open **Git Bash** or **PowerShell** and run:
```bash
git --version
git config --global user.name "YourGitHubUsername"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

---

## 2️⃣ Generate a New SSH Key
In **Git Bash**, run:
```bash
ssh-keygen -t ed25519 -C "you@example.com"
```
- Press **Enter** to confirm the default path (`~/.ssh/id_ed25519`).
- Optionally set a **passphrase** (for extra security) or press Enter to skip.
  - If set, you'll be asked for it later—don't forget it!

> If `ed25519` isn't supported, use:
```bash
ssh-keygen -t rsa -b 4096 -C "you@example.com"
```

---

## 3️⃣ Start SSH Agent and Add Your Key
⚠️ **Choose the section that matches your shell:**

### Git Bash:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### PowerShell (run as Administrator):
```powershell
Start-Service ssh-agent
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

---

## 4️⃣ Copy Your Public Key
In **Git Bash** or **PowerShell**, run:
```bash
cat ~/.ssh/id_ed25519.pub
```
- Copy the full single-line key that starts with `ssh-ed25519`.

---

## 5️⃣ Add the SSH Key to GitHub
1. Go to [GitHub → Settings → SSH and GPG keys](https://github.com/settings/keys)
2. Click **"New SSH key"**
3. Enter a title (e.g., `My Windows PC`)
4. Paste your key into the *Key* field
5. Click **"Add SSH key"**

---

## 6️⃣ Test the SSH Connection
```bash
ssh -T git@github.com
```
**Expected output:**
```
Hi YourGitHubUsername! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 7️⃣ Connect or Push to Your Repository

**Clone a Repo via SSH:**
```bash
git clone git@github.com:YourGitHubUsername/your-repo.git
```

**Initialize and Push a New Repo:**
```bash
cd path/to/your-project
git init
echo "# your-repo" > README.md
git add .
git commit -m "Initial commit"
git remote add origin git@github.com:YourGitHubUsername/your-repo.git
git branch -M main
git push -u origin main
```

**Change Remote from HTTPS to SSH:**
```bash
git remote set-url origin git@github.com:YourGitHubUsername/your-repo.git
```

---

## 8️⃣ Troubleshooting

**Permission denied (publickey)**  
- Verify the key is added to GitHub  
- Check loaded keys:
  ```bash
  ssh-add -l
  ```
- Ensure the SSH agent is running
- Try verbose mode for debugging:
  ```bash
  ssh -vT git@github.com
  ```

**Asked for password when testing SSH?**  
- You're likely using an HTTPS remote. Fix it:
  ```bash
  git remote -v
  git remote set-url origin git@github.com:YourGitHubUsername/your-repo.git
  ```

**SSH Agent won't start in PowerShell?**
- Run PowerShell **as Administrator**
- If still failing, check:
  ```powershell
  Get-Service ssh-agent
  ```

---

## 9️⃣ Useful Commands (Cheat Sheet)
List SSH keys loaded into agent:
```bash
ssh-add -l
```

Show your public key:
```bash
cat ~/.ssh/id_ed25519.pub
```

Test SSH connection to GitHub:
```bash
ssh -T git@github.com
```

Check git remote URLs:
```bash
git remote -v
```

---

✅ **Done!**  
You can now use SSH for cloning, pushing, and pulling from your GitHub repositories securely and without re-entering credentials.