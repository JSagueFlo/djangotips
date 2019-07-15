### settings.py

        USE_I18N = True
        ...
        MIDDLEWARE_CLASSES = (
            ... # This allows to automatically translate urls
            'django.middleware.locale.LocaleMiddleware',
        )


### urls.py
        # prefix with language_code before rest of args: domain.com/en/...
        from django.urls import include, path
        from django.conf.urls.i18n import i18n_patterns
        from django.contrib import admin
        from generic.views import HomeView
        ...
        urlpatterns = [ # NOT TRANSLATED URLS
            path('i18n/', include('django.conf.urls.i18n')),
            ...
            path('',
                 HomeView.as_view(),
                 name="home"),
        ]
        ...
        urlpatterns += i18n_patterns( # TRANSLATED URLS
            path('', HomeView.as_view(), name="home"),
            path('admin/', admin.site.urls, name="admin_site"),
            path('manager/', include('propietari.urls')),
            path('driver/', include('conductor.urls')),
        )
        //Add the urls with normal format, but adding them in the call to i18n_patterns.


### Access current language or a list of languages
        {% get_current_language as LANGUAGE_CODE %}
        {% get_available_languages as LANGUAGES %}


### Links redirect to the corresponding language ones
  With GET it works fine without indicating anything, with POST it turns it into a GET and we lose data.
    - SOLUTION: 
            {% get_current_language as LANGUAGE_CODE %}
            <a href="/{{ LANGUAGE_CODE }}/terms/">{% trans "Terms and Conditions" %}</a>
    - BETTER SOLUTION, put all urls in templates as:
            {% url 'name-in-urls.py' %} -> Links correctly to the selected language


### Redirect to current page after changing language
        <form action="/i18n/setlang/" method="post" style ='float: left; padding: 5px;'>
              {% csrf_token %}
              <input name="next"
                     type="hidden"
                     value="{{ request.path|slice:'3:' }}" />
              <input name="language" type="hidden" value="es" />
              <input type="image"
                     value="Spanish"
                     src="{% static "spain.jpg" %}"
                     height="20"
                     width="30" />
        </form>


### Templates that need internationalization
    Add {% load i18n %} to any template with traducible text  
    NOTE: It doesn't inherit between templates!!!


### Strings in templates that need translating*
        {% trans "name" %}
        {% blocktrans %} Text to be translated {% endblocktrans %}


### Mark things as "translatable" in Python
        from django.utils.translation import ugettext as _
        str_var = _("This string must be translated")


### Plurals
	    from django.utils.translation import ungettext
	    str_var = ungettext(
                'there is %(count)d object',
                'there are %(count)d objects',
	            count) % {
	            'count': count,
	        }
            text = ungettext(
                'There is %(count)d %(name)s object available.',
                'There are %(count)d %(name)s objects available.',
                count
            ) % {
                'count': count,
                'name': Report._meta.verbose_name,
            }

### Create translate files
    In either project root or application root
        mkdir locale
        django-admin.py makemessages -l <language_code>
    Language code is only the first part: es, en, de (not es_ES, en_US, en_EN)
    In locale/<language_code>/LC_MESSAGES/ there will be a .po file. It contains all the strings to be translated.


### Compile translate files
        django-admin.py compilemessages -> Generates .mo file (compiled .po)
