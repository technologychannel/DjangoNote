### Django Models
A model in Django is a Python Class that represent database table. Each attribute in the model represent field in that table.

### How Models Work
- Django use ORM [Object Relational Mapping], which allows you to interact with database using python code instead of SQL.
- Models Define Structure of database, Django automatically create SQL to create database tables and interact with data.

### How To Create
Lets suppose we just create blog app in our project. Go to 'yourappname/models.py' and paste the code:
```python
from django.db import models

# Create your models here.
class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title
```

### Migration
- Migratons is Django way of applying change to database schea. 
- When you define or update model, you create migration to reflect those changes in the database.

### Create Migration
```bash
python manage.py makemigrations
```
This will generate migration file in your apps migration directory.

### Apply Migration
- To apply the migration and update in database run:
```bash
python manage.py migrate
```
This will automaticaly create necessary database table based on your model defination.

### Register Mode To Admin Site
Go to admin.py in your app directory.
```python
from django.contrib import admin
from .models import Post

# Register your models here.
admin.site.register(Post)
```

### Create Superuser
To create superuser use this command:
```bash
python manage.py createsuperuser
```
Now enter username, password, email and open
http://127.0.0.1:8000/admin/
- Enter your username and password
- Now you can add, update, delete, view post.
