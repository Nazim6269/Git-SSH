# 🔐 GitHub SSH Setup Guide (Nushell + Linux)

This guide documents how to set up GitHub SSH authentication in a Nushell environment and fix common issues like `Permission denied (publickey)`.

---

# 🧠 Overview

- Uses SSH keys instead of passwords
- Works with GitHub securely
- Compatible with Nushell (no `eval`)
- Fixes common SSH agent issues

---

# ⚙️ 1. Check existing SSH keys

```bash
ls ~/.ssh
```

Look for:
- `id_ed25519`
- `id_ed25519.pub`

---

# 🔑 2. Generate SSH key (if needed)

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Press Enter for default location.

---

# 🚀 3. Start SSH agent (Nushell)

```bash
ssh-agent
```

You will get output like:

```
SSH_AUTH_SOCK=...; export SSH_AUTH_SOCK;
SSH_AGENT_PID=...; export SSH_AGENT_PID;
```

Now manually set:

```nu
$env.SSH_AUTH_SOCK = "/path/from/output"
$env.SSH_AGENT_PID = 12345
```

---

# ➕ 4. Add SSH key

```bash
ssh-add ~/.ssh/id_ed25519
```

Verify:

```bash
ssh-add -l
```

Expected:

```
256 SHA256:... (ED25519)
```

---

# 🌐 5. Add key to GitHub

Copy key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Then go to:

👉 https://github.com/settings/keys

Steps:
- Click **New SSH Key**
- Paste key
- Save

⚠️ Make sure you're logged into the correct GitHub account.

---

# 🧪 6. Test SSH connection

```bash
ssh -T git@github.com
```

Expected:

```
Hi <username>! You've successfully authenticated...
```

---

# ❌ Common Issues

## Permission denied (publickey)

Fix:
- Add correct `.pub` key to GitHub
- Ensure correct account
- Run:

```bash
ssh-add ~/.ssh/id_ed25519
```

---

## Debug mode

```bash
ssh -vT git@github.com
```

Check for:
- "Offering public key"
- Authentication success

---

# 🔁 7. Clone repo

```bash
git clone git@github.com:username/repo.git
```

---

# 🧠 Important Notes

- SSH email label is NOT used for authentication
- GitHub only checks public key fingerprint
- Nushell does NOT support `eval "$(ssh-agent -s)"`

---

# 🎯 Summary

1. Create SSH key
2. Start ssh-agent (Nushell)
3. Add key to agent
4. Add public key to GitHub
5. Test connection

Done ✅
