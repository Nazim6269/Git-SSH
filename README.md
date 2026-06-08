# 🛠️ Fix: GitHub SSH Permission Denied (publickey)

## 📌 Overview

If you encounter the following error while cloning or pushing to a GitHub repository:

```bash
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

It means GitHub is unable to authenticate your machine using SSH keys.

---

# ✅ Step-by-Step Fix

## 1. Check existing SSH keys

```bash
ls -al ~/.ssh
```

Look for:

- `id_ed25519` and `id_ed25519.pub`
- OR `id_rsa` and `id_rsa.pub`

### ✔ If present:
You already have an SSH key.

### ❌ If not:
Proceed to Step 2.

---

## 2. Generate a new SSH key

```bash
ssh-keygen -t ed25519 -C "your-email@gmail.com"
```

Press **Enter** for all prompts to accept defaults.

This creates:
- Private key: `~/.ssh/id_ed25519`
- Public key: `~/.ssh/id_ed25519.pub`

---

## 3. Start SSH agent and add key

### ⚠️ Bash / Zsh:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### ⚠️ Nushell (if applicable):

```nu
ssh-agent
```
you seee something like
```bash
SSH_AUTH_SOCK=/home/nazimuddin/.ssh/agent/s.4MhcpJe51A.agent.hehFCjUtta; export SSH_AUTH_SOCK;
SSH_AGENT_PID=18043; export SSH_AGENT_PID;
echo Agent pid 18043;
```
then enter
```bash
$env.SSH_AUTH_SOCK = "<value-from-output>"
$env.SSH_AGENT_PID = <pid>
```
then enter
```bash
ssh-add ~/.ssh/id_ed25519
```

---

## 4. Add SSH key to GitHub

Copy your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Then:

1. Go to GitHub → Settings  
2. Navigate to **SSH and GPG keys**  
3. Click **New SSH key**  
4. Paste the key  
5. Save

---

## 5. Test SSH connection

```bash
ssh -T git@github.com
```

### Expected output:

```bash
Hi <username>! You've successfully authenticated...
```

---

## 6. Clone or push repositories

### Clone:

```bash
git clone git@github.com:USERNAME/REPOSITORY.git
```

### Push:

```bash
git push
```

---

# 🔁 Alternative: Use HTTPS instead of SSH

If you prefer not to use SSH:

```bash
git clone https://github.com/USERNAME/REPOSITORY.git
```

---

# 🧠 Important Notes

- SSH authentication depends on the **public key registered in GitHub**, not the email inside the key.
- The email in `ssh-keygen -C` is only a label.
- Multiple SSH keys may cause conflicts if not managed properly.
- Nushell does not support `eval "$(ssh-agent -s)"` natively.

---

# 🎯 Summary

1. Verify or generate SSH key  
2. Start SSH agent  
3. Add key to agent  
4. Register public key in GitHub  
5. Test connection  
6. Use SSH URLs for Git operations  

---
