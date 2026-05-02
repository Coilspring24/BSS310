Assuming this is **GitHub** and your laptop is **Arch/Linux**, do this.

## 1. Check Git is installed

```bash
git --version
```

If missing:

```bash
sudo pacman -S git openssh
```

## 2. Set your Git identity on this laptop

Use your GitHub noreply email if you do not want your personal email exposed.

```bash
git config --global user.name "Your GitHub Username"
git config --global user.email "YOUR_ID+YOUR_USERNAME@users.noreply.github.com"
```

Check:

```bash
git config --global --list
```

## 3. Generate an SSH key

```bash
ssh-keygen -t ed25519 -C "YOUR_ID+YOUR_USERNAME@users.noreply.github.com"
```

When asked where to save it, press **Enter**:

```text
/home/youruser/.ssh/id_ed25519
```

Set a passphrase or leave blank. Passphrase is safer.

## 4. Start SSH agent and add the key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## 5. Copy your public key

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the full output. It starts with:

```text
ssh-ed25519 ...
```

## 6. Add the key to GitHub

Go to:

```text
GitHub → Settings → SSH and GPG keys → New SSH key
```

Title:

```text
ThinkPad Arch Laptop
```

Key:

```text
paste the contents of id_ed25519.pub
```

Save.

## 7. Test SSH connection

```bash
ssh -T git@github.com
```

First time, type:

```text
yes
```

Expected result:

```text
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

That means SSH works.

## 8. Clone your friend's repo using SSH

Use the SSH URL, not HTTPS.

```bash
git clone git@github.com:FRIEND_USERNAME/REPO_NAME.git
```

Then:

```bash
cd REPO_NAME
```

## 9. Push test

Create a branch first. Do not push straight to main unless that is allowed.

```bash
git switch -c test-laptop-setup
echo "SSH test" > ssh-test.txt
git add ssh-test.txt
git commit -m "Test SSH push from laptop"
git push -u origin test-laptop-setup
```

Then open a pull request on GitHub.

## If you already cloned with HTTPS

Check remote:

```bash
git remote -v
```

If it looks like this:

```text
https://github.com/...
```

Change it to SSH:

```bash
git remote set-url origin git@github.com:FRIEND_USERNAME/REPO_NAME.git
```

Verify:

```bash
git remote -v
```

## Useful permanent SSH config

Create/edit:

```bash
nano ~/.ssh/config
```

Add:

```sshconfig
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    AddKeysToAgent yes
```

Fix permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/config
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

## Common failure

If this works:

```bash
ssh -T git@github.com
```

but push fails with permission denied, then your friend probably added the wrong GitHub account, or you only have read access. They must add your GitHub username as a collaborator with write access.
Press **Enter**.

That accepts the default path:

```text
/home/captain/.ssh/id_ed25519
```

Then it will ask for a passphrase:

```text
Enter passphrase (empty for no passphrase):
```

You can either:

```text
press Enter
```

for no passphrase, or type a passphrase if you want extra security.

After that, run:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```

Copy the output from the last command into GitHub:

```text
GitHub → Settings → SSH and GPG keys → New SSH key
```
