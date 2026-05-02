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
