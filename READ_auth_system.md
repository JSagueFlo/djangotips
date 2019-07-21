# Overiding -auth- module

### This will help you handle your custom register-login-logout system

*If doubts, see tutorial:*  
*https://github.com/wsvincent/django-auth-tutorial*  
*Tutorial login and logout custom: https://wsvincent.com/django-user-authentication-tutorial-login-and-logout/*  
*Tutorial simple register (signup): https://wsvincent.com/django-user-authentication-tutorial-signup/*  
*Tutorial password reset: https://wsvincent.com/django-user-authentication-tutorial-password-reset/*  

### Setup all in one:

1. It's recommended, not mandatory, to create an 'accounts' app to handle this stuff

		python manage.py createapp accounts

	Don't forget to add your custom app to INSTALLED_APPS in settings.py

		INSTALLED_APPS = [
		    'django.contrib.admin',
		    'django.contrib.auth',
		    'django.contrib.contenttypes',
		    'django.contrib.sessions',
		    'django.contrib.messages',
		    'django.contrib.staticfiles',
		    # Override auth
		    'accounts.apps.AccountsConfig',
		]

2. Project URLS
		
		# from django.urls import include, path
		# ...

		urlpatterns = [
			# ...
			path('accounts/', include('accounts.urls')),  # Your custom views
    		path('accounts/', include('django.contrib.auth.urls')),  # Login system
    	]

3. accounts URLS

		from django.urls import path
		from . import views

		urlpatterns = [
		    path('signup/', views.SignUp.as_view(), name='signup'),
		]

4. accounts VIEWS

		from django.contrib.auth.forms import UserCreationForm
		from django.urls import reverse_lazy
		from django.views import generic

		class SignUp(generic.CreateView):
		    form_class = UserCreationForm
		    success_url = reverse_lazy('login')
		    template_name = 'signup.html'

5. Templates

	**NOTE:** This Signup view is a very basic one. Check out for more detailed registration process.
	./templates/signup.html
		```
		{% extends '_base.html' %}
		{% load static %}
		{% block title %} - Signup{% endblock %}
		{% block body %}
		<div class="container">
			<div class="section">
				<h1>Sign Up</h1>
				<form method="post">
					{% csrf_token %}
					{{ form.as_p }}
					<button type="submit">Sign Up</button>
				</form>
			</div>
		</div>
		{% endblock %}
		```

	./templates/registration/login.html
		```
		{% extends '_base.html' %}
		{% load static %}
		{% block title %} - Login{% endblock %}
		{% block body %}
		<div class="container">
			<div class="section">
				<h1>Login</h1>
				<form method="post">
					{% csrf_token %}
					{{ form.as_p }}
					<button type="submit">Login</button>
				</form>
			</div>
		</div>
		{% endblock %}
		```

	**NOTE:** For some reason this template **MUST BE** in project **root directory** 'templates'
	./templates/registration/password_reset_complete.html
		```
		{% extends '_base.html' %}
		{% load static %}
		{% block title %} - Password reset complete1{% endblock %}
		{% block body %}
		<div class="container">
			<div class="section">
				<h1>Password reset complete</h1>
				<p>Your new password has been set. You can log in now on the <a href="{% url 'login' %}">log in page</a>.</p>
			</div>
		</div>
		{% endblock %}
		```

	**NOTE:** For some reason this template **MUST BE** in project **root directory** 'templates'
	./templates/registration/password_reset_confirm.html
		```
		{% extends '_base.html' %}
		{% load static %}
		{% block title %} - Enter new password{% endblock %}
		{% block body %}
		<div class="container">
			<div class="section">
				<h1>Set a new password!</h1>
				<form method="POST">
					{% csrf_token %}
					{{ form.as_p }}
					<input type="submit" value="Change my password">
				</form>
			</div>
		</div>
		{% endblock %}
		```

	**NOTE:** For some reason this template **MUST BE** in project **root directory** 'templates'
	./templates/registration/password_reset_done.html
		```
		{% extends '_base.html' %}
		{% load static %}
		{% block title %} - Email Sent{% endblock %}
		{% block body %}
		<div class="container">
			<div class="section">
				<h1>Check your inbox.</h1>
				<p>We've emailed you instructions for setting your password. You should receive the email shortly!</p>
			</div>
		</div>
		{% endblock %}
		```

	**NOTE:** For some reason this template **MUST BE** in project **root directory** 'templates'
	./templates/registration/password_reset_form.html
		```
		{% extends '_base.html' %}
		{% load static %}
		{% block title %} - Reset password{% endblock %}
		{% block body %}
		<div class="container">
			<div class="section">
				<h1>Forgot your password?</h1>
				<p>Enter your email address below, and we'll email instructions for setting a new one.</p>
				<form method="post">
					{% csrf_token %}
					{{ form.as_p }}
					<button type="submit">Send me instructions!</button>
				</form>
			</div>
		</div>
		{% endblock %}
		```
