# Databases

## Using PostgreSQL

1. Install PostgreSQL on your machine
2. Run it and create your own database (default owner postgres)
3. Connect that database to your django project

### settings.py
		DATABASES = {
		    'default': {
		        'ENGINE': 'django.db.backends.postgresql',
		        'NAME': 'database_name',
		        'USER': 'postgres',
		        'PASSWORD': 'your_password',
		        'HOST': '127.0.0.1',
		        'PORT': '5432',
		    }
		}


### Deploying in Heroku (automatically)
		# At the beggining of settings.py
		import django_heroku
		# As last line in settings.py
		# Activate Django-Heroku.
		django_heroku.settings(locals())