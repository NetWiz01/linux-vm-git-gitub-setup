# Linux VM: Git & GitHub Setup Guide

This guide explains how to configure Git and GitHub access on a Linux VM so that repositories can be cloned, committed to, and pushed to GitHub

## 1. Check whether Git is installed
```text
git --version
```
if it isn't installed:
# Ubuntu/Debian
```text
sudo apt update
sudo apt install git -y
```
Then verify:
```text
git --version
```
## 2. Configure your Git Identity
Use the name and email you want associated with your commits:
```text
git config --global user.name "Your Name"
git config --global user.email "your-github-email@example.com"
```
Check it:
```text
git config --global --list
```
You should see something like:
```text
user.name=Your Name
user.email=your-github-email@example.com
```
## 3. Check whether you already have a SSH key
Before creating a new one:
```text
ls -la ~/.ssh
```
Look for files such as:
```text
id_ed25519
id_ed25519.pub
```
If this is a new VM, you probably won't have them.

## 4. Generate a SSH key
I recommend **Ed25519**:
```text
ssh-keygen -t ed25519 -C "your-github-email@example.com"
```
At the time of writing, GitHub currently documents Ed25519 as the standard option for generating a new SSH key.

When prompted:
```text
Enter file in which to save the key (/home/yourusername/.ssh/id_ed25519):
```
Press **Enter** to accept the default.

Then you will be asked for a passphrase.

I recommend using one, especially if this VM contains valuable code.

You should now have:
```text
~/.ssh/id_ed25519       ← Private key (NEVER share this)
~/.ssh/id_ed25519.pub   ← Public key (this goes into GitHub)
```

## 5. Start the SSH agent and add your key
Run:
```text
eval "$(ssh-agent -s)"
```
Then:
```text
ssh-add ~/.ssh/id_ed25519
```

## 6. Copy your public key
Display it:
```text
cat ~/.ssh/id_ed25519.pub
```
You will see something starting with:
```text
ssh-ed25519 AAAAC3...
```
Copy the entire line.

⚠️ Only copy the .pub key.

**Never share this file**:
```text
~/.ssh/id_ed25519
```

## 7. Add the SSH key to your GitHub account
In GitHub:

**Profile picture → Settings → SSH and GPG keys → New SSH key**

Give it a useful name such as:
```text
Linux VM
```
Choose **Authentication Key** and paste your public key.

GitHub's instructions confirm that the public key must be added to your account to enable SSH authentication.

## 8. Test the connection
Back on your Linux VM:
```text
ssh -T git@github.com
```
The first time, you may see:
```text
Are you sure you want to continue connecting?
```
Type:
```text
yes
```
If everything works, you will get a message similar to:
```text
Hi YOUR-USERNAME! You've successfully authenticated...
```
At that point, your VM is connected to your GitHub account. 🎉

## 9. Create a repository and push code
For example, let's say your project is here:
```text
cd ~/my-project
```
Initialize Git:
```text
git init -b main
```
Check what's there:
```text
git status
```
Add your files:
```text
git add .
```
Create your first commit:
```text
git commit -m "Initial commit"
```
##


### Create an empty repository on GitHub

On GitHub, create a new repository, for example:
```text
my-project
```
**Don't initialise it with a README** if you are pushing an existing project, as that keeps the first push simpler.

GitHub will give you an SSH URL similar to:
```text
git@github.com:YOUR-USERNAME/my-project.git
```
Add it as your remote:
```text
git remote add origin git@github.com:YOUR-USERNAME/my-project.git
```
Set your branch to ` main `:
```text
git branch -M main
```
Push:
```bash
git push -u origin main
```
###

### Your normal workflow afterwards

Once everything is configured, your typical workflow will simply be:
```bash
git status
```
```bash
git add .
```
```bash
git commit -m "Describe what you changed"
```
```bash
git push
```
And to get changes from GitHub:
```bash
git pull
```
###





