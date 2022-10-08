# Django oAuth2

Authentication local and social accounts with oAuth2

## Configurations inside project:

### Settings.py

* Templates settings.

```shell
# Subdir Templates
TEMPLATES_DIRS = os.path.join(BASE_DIR, "templates")
```

* Django Allauth settings.

```pycon
INSTALLED_APPS = [
    # APPS already installed here
    
    # Allauth - authentication
    "allauth",
    "allauth.account",
    "allauth.socialaccount",
    # Providers (allauth)
    "allauth.socialaccount.providers.linkedin_oauth2",
    "allauth.socialaccount.providers.github",
]
```

```pycon
TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [TEMPLATES_DIRS],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                # Already defined Django-related contexts here

                # `allauth` needs this from django
                "django.contrib.auth.context_processors.auth",
            ],
        },
    },
]
```

```pycon
# Authentication Backend

AUTHENTICATION_BACKENDS = [
    # Needed to log-in by username in Django admin, regardless of `allauth`
    'django.contrib.auth.backends.ModelBackend',

    # `allauth` specific authentication methods, such as login by e-mail
    'allauth.account.auth_backends.AuthenticationBackend',
]
```

```pycon
# Sites

SITE_ID = 1
```

```pycon
# Configurations (Allauth)

ACCOUNT_AUTHENTICATION_METHOD = "email"
ACCOUNT_EMAIL_REQUIRED = True
ACCOUNT_EMAIL_VERIFICATION = "mandatory"
ACCOUNT_USERNAME_REQUIRED = False
SOCIALACCOUNT_AUTO_SIGNUP = False  # to make the user fill in all required fields
```

```pycon
# Providers (Allauth)
# Scopes define what your app can do on a user's behalf.

SOCIALACCOUNT_ADAPTER = 'allauth.socialaccount.adapter.DefaultSocialAccountAdapter'
SOCIALACCOUNT_PROVIDERS = {
    "linkedin": {
        "SCOPE": ["r_basicprofile", "r_emailaddress"],
        "PROFILE_FIELDS": [
            "first-name",
            "last-name",
            "email-address",
        ],
    },
    "github": {
        "SCOPE": [
            "user",
        ],
    },
}
```

* Email Backend settings

```pycon
# Email
# https://docs.djangoproject.com/en/4.1/topics/email/#console-backend

EMAIL_BACKEND = "django.core.mail.backends.console.EmailBackend"
```

* URL Redirect settings

````shell
# Redirect URL

LOGIN_REDIRECT_URL = "/profile"
````

### Templates

Copy ``account``, ``openid``, ``socialaccount`` folders from ``allauth.templates``, than paste to your templates.

## Configurations outside project

### LinkedIn:

Link: https://developer.linkedin.com/

### GitHub:

Link: https://github.com/settings/developers

* Client ID = ``YOUR_CLIENT_ID``
* Client Secret = ``YOUR_SECRET_KEY``
* Authorized callback/redirect URLs: ``'http://127.0.0.1:8000/accounts/linkedin_oauth2/login/callback/'``

### Create Super User

````shell
$ poetry run python manage.py migrate
$ poetry run python manage.py createsuperuser
$ poetry run python manage.py runserver 
````

### Django Admin:

* Sites

```pycon
# Localhost for debug purposes.
Domain name: 127.0.0.1:8000
Display name: 127.0.0.1:8000
```

* Social Application

```pycon
# A GitHub provider example.
Provider: GitHub
Name: 'example_GitHub'
Client id: 'YOUR CLIENT ID'
Secret key: 'YOUR SECRET KEY'
Key: 'LEAVE BLANK'
Sites: 127.0.0.1:8000
```
