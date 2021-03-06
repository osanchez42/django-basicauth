================
django-basicauth
================

Basic auth utilities for Django.

Requires
========

Tested under...

* Python

  * 2.7
  * 3.6
  * 3.7

* Django

  * 1.11
  * 2.1
  * 2.2

Installation
============
NEEDS TO BE UPDATED, WILL NOT PROVIDE CHANGES

::

    pip install django-basicauth


Usage
=====

.. code-block:: python

    from basicauth.decorators import basic_auth_required

    @basic_auth_required
    def myview(request):
        ...

or by a middleware.

.. code-block:: python

    MIDDLEWARE = (
        'basicauth.middleware.BasicAuthMiddleware',
        ...
    )

The name of ``MIDDLEWARE`` settings is ``MIDDLEWARE_CLASSES`` on Django 1.8.

Basic Auth for specific requests only
-------------------------------------

To apply basic auth for specific requests,
Use ``target_test`` argument.

In the below code, anonymous users will be required Basic Auth
Authenticated users can pass it without `Basic ...` header.

.. code-block:: python

    from basicauth.decorators import basic_auth_required

    @basic_auth_required(
        target_test=lambda request: not request.user.is_authenticated
    )
    def myview(request):
        ...

``target_test`` accepts ``typing.Callable[[HttpRequest], bool]``,
and if the callable returns ``True``, Basic Auth will be required.

Applying decorator to CBVs
==========================

To apply ``@basic_auth_required`` decorator to Class Based Views,
use ``django.utils.decorators.method_decorator``.

.. code-block:: python

    from django.utils.decorators import method_decorator
    from basicauth.decorators import basic_auth_required

    @method_decorator(basic_auth_required, name='dispatch')
    class YourView(TemplateView):
        template_name = "my-template.html"

Settings
========

* ``BASICAUTH_REALM``: realm string, default is "Secure resource".
* ``BASICAUTH_DISABLE``: Disable all of barriers by this library.

Changes
========
django does not accept basic authentication headers to be passed in without
using it to authenticate a user. If a user is authenticated, the authorization headers
is added to the request META and the user object updated. if the user is not
authenticated, the header is stripped and an anonymous user object is added.
This middleware was created to provide an alternative method of authenticating users.
Originally the middleware allowed incoming requests to be authenticated using a dictionary
defined in settings.py. This has been removed, and all incoming requests that contain
authorization headers will remain. May apply to other environments
