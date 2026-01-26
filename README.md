# 🛠️ Fix GitHub SSH Permission Denied (publickey)

#### If you see this error when cloning or pushing a repository:

```bash

git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

```
This means GitHub cannot authenticate your computer using SSH.

# Step-by-Step Solution
1. Check if you already have an SSH key
```
ls -al ~/.ssh

```

Look for files like:
- id_ed25519 and id_ed25519.pub
- or id_rsa and id_rsa.pub

If you see them, you already have a key.
If not, continue to step 2.

### 2. Generate a new SSH key
```
ssh-keygen -t ed25519 -C "your-email@gmail.com"

```
Press Enter for all prompts.

### 3. Start SSH agent and add your key
```

eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

```

### 4. Add SSH key to your GitHub account

Copy your public key:
```
cat ~/.ssh/id_ed25519.pub

```

Then:
Go to GitHub → Settings
SSH and GPG keys
Click New SSH key
Paste the key
Save


### 5. Test SSH connection
```
ssh -T git@github.com

```
Expected output:
Hi <your-username>! You've successfully authenticated...


### 6. Clone or push again
```
git clone git@github.com:YOUR_USERNAME/YOUR_REPO.git

```

or
```
git push

```
### Alternative: Use HTTPS Instead of SSH
If you don’t want to use SSH:
```
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git

```
