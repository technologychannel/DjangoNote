### Host Django Website On VPS Using DigitalOcean. Website Include Django With Sqlite3 Database.

### Step 1: Create Droplet on DigitalOcean
### Step 2: Connect to Droplet using SSH
```
ssh root@your_droplet_ip
```
### Step 3: Update and Upgrade Packages
```bash
sudo apt update && sudo apt upgrade -y
```

### Step 4: Install Python, Pip, and Virtual Environment
```bash
sudo apt install python3-pip python3-dev libpq-dev nginx curl -y
sudo apt install python3-virtualenv -y
```

### Step 5: Create a New Directory for Django Project
```bash
mkdir django_project
cd django_project
```

### Step 6: Clone Django Project from GitHub
```bash
git clone your_project_url
```

### Step 7: Create Virtual Environment
```bash
python3 -m virtualenv venv
source venv/bin/activate
```

### Step 8: Install Gunicorn
```bash
pip install gunicorn
```

### Step 9: Test Gunicorn
```bash
gunicorn --bind 0.0.0.0:8000 yourproject.wsgi
```

### Step 10: Create a systemd Service File
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

Demo
```ini
[Unit]
Description=gunicorn daemon for mybiodata
After=network.target

[Service]
User=yogesh
Group=www-data
WorkingDirectory=/home/yogesh/djangoproject/mybiodata
ExecStart=/home/yogesh/djangoproject/venv/bin/gunicorn --workers 3 --bind unix:/home/yogesh/djangoproject/mybiodata/mybiodata.sock yogesh_bio.wsgi:application

[Install]
WantedBy=multi-user.target
```

### Step 11: Reload and Enable Gunicorn Service
```bash
sudo systemctl start gunicorn
sudo systemctl enable gunicorn
```

### Step 12: Configure Nginx
```bash
sudo nano /etc/nginx/sites-available/yourproject
```
Demo
```bash
sudo nano /etc/nginx/sites-available/mybiodata
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

Demo 
```bash
sudo nano /etc/nginx/sites-available/mybiodata
```

```ini
server {
    listen 80;
    server_name yogeshbharati.com www.yogeshbharati.com;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/yogesh/djangoproject/mybiodata;
    }
    location /media/ {
        root /home/yogesh/djangoproject/mybiodata;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/home/yogesh/djangoproject/mybiodata/mybiodata.sock;
    }
}
```


### Step 13: Enable Nginx Configuration
```bash
sudo ln -s /etc/nginx/sites-available/yourproject /etc/nginx/sites-enabled
sudo nginx -t
sudo systemctl restart nginx
```

### Step 14: Secure With SSL
```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d your_domain.com -d www.your_domain.com
```

### Step 15: Update Django Settings
```python
ALLOWED_HOSTS = ['your_domain.com', 'www.your_domain.com']
```

### Step 16: Migrate Database
```bash
python manage.py migrate
```

### Step 17: Test Website
```bash
python manage.py runserver
```

### Step 18: Done





