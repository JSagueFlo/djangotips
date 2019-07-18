# Basic Django Project

### Using Python 3.6.8 and Django 2.2.3

1. Create root directory for your project
		
		mkdir project_root_directory
		cd project_root_directory

2. Create a virtual enviroment to install dependencies locally

		python -m venv venv
		venv\Scripts\activate

3. Install your project dependencies

		pip install -r requirements.txt

	or

		pip install dependency_name

4. Create your project

		django-admin startproject project_name
		cd project_name

5. Create a generic app to get started

		python manage.py startapp generic_app

6. Set your database
	
	Default is SQLite, but if you want to use PostgreSQL modify settings.py as follows:

		DATABASES = {
		    'default': {
		        'ENGINE': 'django.db.backends.postgresql',
		        'NAME': 'project_name',
		        'USER': 'postgres',
		        'PASSWORD': 'password',
		        'HOST': '127.0.0.1',
		        'PORT': '5432',
		    }
		}
		
7. 
8. 
9. for PostgreSQL modify settings.py as follows:
		
		aklsfjd
		asdf

10. python manage.py makemigrations
11. python manage.py migrate
12. python manage.py createsuperuser
