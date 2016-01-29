# django-rest-registration

User registration REST API, based on django-rest-framework.

WARNING: `django-rest-registration` is only Python 3 compatible.


## Features

* Supported views:
    * registration (sign-up) with verification
    * login/logout (sign-in), session- or token-based
    * user profile
    * reset password
    * change password
* Modeless (uses cryptographic signing instead of profile models)


## Installation

You can install `django-rest-registration` via pip:

    pip install git+https://github.com/szopu/django-rest-registration

Then, you should add it to the `INSTALLED_APPS` so the app templates
for notification emails can be accessed:

    INSTALLED_APPS=(
        ...

        'rest_registration',
    ),

After that, you can use the urls in your urlconfig, for instance:

    api_urlpatterns = [
        ...

        url(r'accounts/', include('rest_registration.api.urls')),
    ]


    urlpatterns = [
        ...

        url(r'^api/v1/', include(api_urlpatterns)),
    ]


## Configuration


You can configure `django-rest-registraton` using the `REST_REGISTRATION`
setting in your django settings (similarly to `django-rest-framework`).
The default values are:

    REST_REGISTRATION = {
        'USER_LOGIN_FIELDS': None,
        'USER_HIDDEN_FIELDS': (
            'is_active',
            'is_staff',
            'is_superuser',
            'user_permissions',
            'groups',
            'date_joined',
        ),
        'USER_PUBLIC_FIELDS': None,
        'USER_EMAIL_FIELD': 'email',

        'USER_VERIFICATION_FLAG_FIELD': 'is_active',

        'REGISTER_SERIALIZER_CLASS': (
            'rest_registration.api.serializers.DefaultRegisterUserSerializer'),

        'REGISTER_VERIFICATION_ENABLED': True,
        'REGISTER_VERIFICATION_PERIOD': datetime.timedelta(days=7),
        'REGISTER_VERIFICATION_URL': None,
        'REGISTER_VERIFICATION_EMAIL_TEMPLATES': {
            'subject':  'rest_registration/register/subject.txt',
            'body':  'rest_registration/register/body.txt',
        },

        'LOGIN_AUTHENTICATE_SESSION': None,
        'LOGIN_RETRIEVE_TOKEN': None,

        'RESET_PASSWORD_VERIFICATION_PERIOD': datetime.timedelta(days=1),
        'RESET_PASSWORD_VERIFICATION_URL': None,
        'RESET_PASSWORD_VERIFICATION_EMAIL_TEMPLATES': {
            'subject': 'rest_registration/reset_password/subject.txt',
            'body': 'rest_registration/reset_password/body.txt',
        },

        'PROFILE_SERIALIZER_CLASS': (
            'rest_registration.api.serializers.DefaultUserProfileSerializer'),

        'VERIFICATION_FROM_EMAIL': None,
        'VERIFICATION_REPLY_TO_EMAIL': None,
    }

The `USER_*` fields can be set directly in the user class
(specified by `settings.AUTH_USER_MODEL`) without using
the `USER_` prefix (`EMAIL_FIELD`, etc.).
