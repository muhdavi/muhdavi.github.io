---
title: Connecting to GitHub with SSH
author: muhdavi
date: 2023-01-15 10:10:00 +0700
categories: [Blogging, Tutorial]
tags: [Git, GitHub, SSH]
render_with_liquid: false
---

This tutorial get from [Chairat Onyaem (Par) Blog](https://pacroy.com/connecting-to-github-with-ssh-f54248ccf30d).

Here is the quick guide to push/pull GitHub repository via SSH connection based on the [instructions on GitHub Help](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

1. Open [Git Bash](https://git-scm.com/downloads)
2. Generate a new SSH key, use command `ssh-keygen -t rsa -b 4096 -C "your_comment_or_email"`. Enter to accept default filename and input passphase as needed.
3. Check if SSH is running withy command `eval $(ssh-agent -s)`. You should see output like: `Agent pid 4056`
4. Add the new SSH private key to SSH agent with command `ssh-add ~/.ssh/id_rsa`
5. Copy the public key to the clipboard with command `clip < ~/.ssh/id_rsa.pub`
6. Go to GitHub [SSH and GPG keys setting](https://github.com/settings/keys)
7. Click **New SSH**, name Title for your reference, and paste into the **Key**
8. Click **Add SSH Key**
9. Test connection with command `ssh -T git@github.com`. If everything works fine, you should see this message
```bash
Hi...! You`ve successfully authenticated
```

Now, you should be able to connect to GitHub via SSH.

### Additional Tips

To generate SSH key files without prompt, use:
```bash
ssh-keygen -t rsa -b 4096 -C "your_comment" -f ~/.ssh/id_rsa -N "passphase"
```
