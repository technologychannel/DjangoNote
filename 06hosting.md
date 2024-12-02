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

### Step 11: Setup WebServer Using Nginx and Gunicorn
```bash
pip install gunicorn
sudo apt install nginx -y
```

### Step 12: Test Gunicorn
```bash
gunicorn --bind 0.0.0.0:8000 yourproject.wsgi
```

### Step 13: Create a systemd Service File
```bash
sudo nano /etc/systemd/system/gunicorn.service
```
```ini
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=django_user
Group=www-data
WorkingDirectory=/home/django_user/yourproject
ExecStart=/home/django_user/yourproject/venv/bin/gunicorn --workers 3 --bind unix:/home/django_user/yourproject/gunicorn.sock yourproject.wsgi:application

[Install]
WantedBy=multi-user.target
```
### Step 14: Reload and Enable Gunicorn Service
```bash
sudo systemctl start gunicorn
sudo systemctl enable gunicorn
```

### Step 15: Configure Nginx
```bash
sudo nano /etc/nginx/sites-available/yourproject
```
```ini
server {
    listen 80;
    server_name your_domain.com www.your_domain.com;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/django_user/yourproject;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/home/django_user/yourproject/gunicorn.sock;
    }
}
```
### Step 16: Enable Nginx Configuration
```bash
sudo ln -s /etc/nginx/sites-available/yourproject /etc/nginx/sites-enabled
sudo nginx -t
sudo systemctl restart nginx
```

### Step 17: Secure With SSL
```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d your_domain.com -d www.your_domain.com
```

### Step 18: Update Django Settings
```python
ALLOWED_HOSTS = ['your_domain.com', 'www.your_domain.com']
```

### Step 19: Migrate Database
```bash
python manage.py migrate
```

### Step 20: Test Website
```bash
python manage.py runserver
```

### Step 21: Done





