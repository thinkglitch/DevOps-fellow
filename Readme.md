# Beginner's Practical Guide – Linux, Networking, and Git

This guide introduces Linux fundamentals, networking basics, and Git with practical examples so beginners can follow along step by step.

---

## 1. Linux Fundamentals

### Navigating the Linux CLI
```bash
pwd                 # Print current directory
ls                  # List files
ls -l               # Detailed listing
cd /home/user       # Go to directory
mkdir test_dir      # Create directory
rmdir test_dir      # Remove directory
```

### File Operations
```bash
touch file.txt      # Create file
cat file.txt        # View contents
cp file.txt copy.txt # Copy file
mv copy.txt moved.txt # Move/Rename file
rm moved.txt        # Delete file
```

### User and Group Management
```bash
id                  # Show current user info
whoami              # Display logged-in user
su otheruser        # Switch user
sudo ls /root       # Run command with admin rights
```

### File Permissions and Security
```bash
ls -l               # Show file permissions
chmod 755 file.sh   # Change file permissions
chown user:group file.sh # Change ownership
```

### Package Management
- **RedHat/CentOS**:
```bash
sudo yum install nginx
sudo yum remove nginx
```
- **Debian/Ubuntu**:
```bash
sudo apt update
sudo apt install nginx
sudo apt remove nginx
```

### Process and Service Management
```bash
systemctl start nginx     # Start service
systemctl stop nginx      # Stop service
systemctl status nginx    # Service status
systemctl enable nginx    # Start on boot
```

### Shell Types & Bash Scripting
```bash
echo $SHELL            # Show current shell
chsh -s /bin/bash      # Change shell to bash
```
**Simple Bash Script (hello.sh):**
```bash
#!/bin/bash
echo "Hello, World!"
```
Run with:
```bash
bash hello.sh
```

### VI Editor
```bash
vi file.txt
# Press 'i' to insert, type text
# Press ESC, then :wq to save & exit
```

### Virtual Machines with Vagrant
```bash
vagrant init centos/7   # Initialize CentOS VM
vagrant up              # Start VM
vagrant ssh             # Connect to VM
```

### JSON & YAML Basics
**JSON Example (config.json):**
```json
{ "name": "server1", "ip": "192.168.1.10" }
```
**YAML Example (config.yaml):**
```yaml
name: server1
ip: 192.168.1.10
```

---

## 2. Networking Basics

### IP and Interfaces
```bash
ip link                # Show network interfaces
ip addr show           # Show IPs
ip addr add 192.168.1.10/24 dev eth0  # Assign IP
```

### Routing and Gateways
```bash
ip route               # Show routing table
ip route add 192.168.2.0/24 via 192.168.1.1  # Add route
ip route add default via 192.168.1.1         # Set default gateway
```

### DNS and Name Resolution
```bash
ping 8.8.8.8           # Ping Google DNS
ping google.com        # Test DNS resolution
sudo nano /etc/hosts   # Add custom DNS entry
192.168.1.11   db
```
```bash
ping db
curl http://db
```

### Enable IP Forwarding
```bash
cat /proc/sys/net/ipv4/ip_forward   # Check
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward # Enable
```

---

## 3. Git Basics

Git is a distributed version control system. Below are key commands with practical usage.

### Initialize and Clone Repositories
```bash
git init                  # Start new repo in current folder
git clone https://github.com/user/repo.git   # Clone repo
```

### Tracking Changes
```bash
git status                # Show status of files
git add file.txt          # Stage file for commit
git commit -m "Added file.txt"  # Commit changes
```

### Sync with Remote
```bash
git push origin main      # Push commits to remote
git pull                  # Fetch & merge changes
```

### History and Logs
```bash
git log                   # Show commit history
git diff                  # Compare changes
```

---
# Git Branching, Pull Requests, and Undoing Mistakes

## Objective
To practice and demonstrate the use of Git branching, committing, pushing, merging via Pull Requests (PRs), and undoing mistakes in Git. This ensures proper collaboration while maintaining stability on the main branch.

---

## Step 1: Initialize Repository
- Created a local Git repository.
- Added initial files and pushed them to the remote main branch.

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin git@github.com:murafay/example.git
git push -u origin main
```

---

## Step 2: Create a Development Branch
- Created a new branch `dev` from main.
- Switched to `dev` for feature development.

```bash
git branch dev
git checkout dev
```

---

## Step 3: Add New Files and Commit
- Edited and added new files (e.g., `compose.yaml`).
- Committed the changes to dev.

```bash
git add compose.yaml
git commit -m "Added compose.yaml for testing"
git push -u origin dev
```

---

## Step 4: Open Pull Request (PR)
- Pushed `dev` to GitHub.
- Created a Pull Request on GitHub from `dev → main`.
- This allowed review and merging changes into the stable branch.

---

## Step 5: Merge Pull Request
- Merged the PR into `main`.

After merge:
- Both `main` and `dev` contained the same files at that commit.
- The changes made in `dev` became part of `main`.

---

## Step 6: Divergence and Syncing
- Added another file to `dev`.
- Now `dev` had new changes not in `main`.
- Opened another PR and merged it, bringing both branches back in sync.

---

## Step 7: Undo Solutions
Sometimes mistakes happen (wrong commit, wrong push, or accidental file addition). Git provides multiple ways to undo:

- **Undo Last Commit (keep changes in working directory):**
```bash
git reset --soft HEAD~1
```
Useful if you want to re-commit with a better message.

- **Undo Last Commit (discard changes):**
```bash
git reset --hard HEAD~1
```
Completely removes the last commit and its changes.

- **Undo a Commit That’s Already Pushed (create a new commit that reverts it):**
```bash
git revert <commit-hash>
git push origin dev
```
Safest way on shared branches since it doesn’t rewrite history.

- **Undo a File Before Commit:**
```bash
git checkout -- filename
```
Restores the file to the last committed version.

- **Undo Pushed Changes (force push – not recommended unless you’re alone):**
```bash
git reset --hard HEAD~1
git push origin dev --force
```
Danger: this rewrites history and can break other collaborators’ work.

---
