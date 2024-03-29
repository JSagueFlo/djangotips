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

6. Set the settings of your project
	
	### Custom apps
		
		INSTALLED_APPS = [
		    'django.contrib.admin',
		    'django.contrib.auth',
		    'django.contrib.contenttypes',
		    'django.contrib.sessions',
		    'django.contrib.messages',
		    'django.contrib.staticfiles',
		    # Custom apps
		    'generic',
		]

	### Templates
	By default, any 'templates' directory in app directory will be read.  
	Adding 'os.path.join(BASE_DIR, 'templates')' as element of 'DIRS' will allow us to create a 'templates' directory in the project root directory (near manage.py) containing more templates.  
	This way, we can overide templates like 'auth' ones (e.g. registration/reset_password_form.html)

		TEMPLATES = [
		    {
		        'BACKEND': 'django.template.backends.django.DjangoTemplates',
		        'DIRS': [os.path.join(BASE_DIR, 'templates')],
		        'APP_DIRS': True,
		        'OPTIONS': {
		            'context_processors': [
		                'django.template.context_processors.debug',
		                'django.template.context_processors.request',
		                'django.contrib.auth.context_processors.auth',
		                'django.contrib.messages.context_processors.messages',
		            ],
		        },
		    },
		]

	### Database
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

	### Static files
	With the following configuration, create a 'static' directory at your project folder, next to 'manage.py' file.
	Add your static files such as .js, .css, .jpg... to this directory

		STATIC_URL = '/static/'
		STATIC_ROOT = os.path.join(BASE_DIR, "staticfiles")
		STATICFILES_DIRS = [os.path.join(BASE_DIR, "static")]

	### Cookie session age

		# Session expires in 5 minutes
		SESSION_COOKIE_AGE = 5 * 60

	### Emailing
	Just in case that you need to handle emails:

		# Emailing system
		# This will fake-send emails. Emails will be shown at console.
		EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'

7. Check for migrations and migrate them to your database

		python manage.py makemigrations
		python manage.py migrate

8. Create a super user to administrate your site

		python manage.py createsuperuser

9. Create a home view

	We will use generic.views to program our views

		from django.shortcuts import render
		# Class based views
		from django.views.generic import TemplateView

		# Create your views here.
		class HomeView(TemplateView):
		    template_name = "home.html"
			
		    def get_context_data(self, **kwargs):
		        context = super().get_context_data(**kwargs)
		        context['test'] = "Home view!"
		        return context

	Notice the template_name "home.html", lets create the template  
	*You can create your home.html template in the root templates directory or create a directory inside generic called 'templates'*  
	*Inside 'templates' create a 'home.html' file*

		```
		{% load static %}
		<html>
		<head>
			<title>Home</title>
			<link rel="stylesheet" href="{% static 'example.css' %}">
		</head>
		<body>
			<h1>Home h1 tag</h1>
		</body>
		</html>
		```

	To publish the HomeView modify project_name.urls.py like this

		from django.contrib import admin
		from django.urls import path
		from generic.views import HomeView

		urlpatterns = [
		    path('admin/', admin.site.urls),
		    path('', HomeView.as_view(), name="home"),
		    # The following path puts all the urls found
		    # in generic.urls.py inside the url domain.com/generic/...
		    # path('generic/', include('generic.urls')),
		]
