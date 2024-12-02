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
### Step 6: Enable Firewall
```bash
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### Step 7: Install Python, Pip, and Virtual Environment
```bash
sudo apt install python3 python3-pip python3-venv -y
```

### Step 8: Create a New Directory for Django Project
```bash
mkdir django_project
cd django_project
```

### Step 9: Clone Django Project from GitHub
```bash
git clone your_project_url
```

### Step 10: Create Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate
```

### Step 11: Install Django and Other Required Packages
```bash
pip install django gunicorn
```
