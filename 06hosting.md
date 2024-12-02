### Host Django Website On VPS Using DigitalOcean. Website Include Django With Sqlite3 Database.

### Step 1: Create Droplet on DigitalOcean
### Step 2: Connect to Droplet using SSH
```
ssh root@your_droplet_ip
```
### Step 3: Update and Upgrade Packages
```
sudo apt update && sudo apt upgrade -y
```

### Step 4: Create a New User (For Security)
```bash
adduser your_username
usermod -aG sudo your_username
```
### Step 5: Switch to new user
```bash
su - your_username
```
### Step 5: Install Python and Pip
```bash

