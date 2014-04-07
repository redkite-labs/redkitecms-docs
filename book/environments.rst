Environments
============

This chapter explains the RedKite CMS environments configuration in detail. 


.. note::

    This is quite a technical chapter but explains a simple yet important concept, so you should
    read this chapter carefully.


What is a work environment?
---------------------------

Symfony2 handles several environments where your website may run. Here is the 
environment definition taken from the Symfony2 book:

    An application can run in various environments. The different environments share the same PHP code
    (apart from the front controller), but use different configuration. 

    For instance, a dev environment will
    log warnings and errors, while a prod environment will only log errors. Some files are rebuilt on each
    request in the dev environment (for the developer's convenience), but cached in the prod environment.
    
    All environments live together on the same machine and execute the same application.

    A Symfony2 project generally begins with three environments (dev, test and prod), though creating new
    environments is easy. 

    You can view your application in different environments simply by changing the
    front controller in your browser. To see the application in the dev environment, access the application
    via the development front controller:

    http://localhost/app_dev.php/hello/Ryan

    If you'd like to see how your application will behave in the production environment, call the prod front
    controller instead:    

    http://localhost/app.php/hello/Ryan
    
They introduced the concept of a **front controller** which is a simple php file that 
defines the unique entry point for the entire application.

Let's explain this concept with examples. Suppose your website has three pages: home, 
about and contacts pages.

To open these pages you will use the following addresses in your browser:

.. code:: text

    http://localhost/[FRONT CONTROLLER]/homepage
    http://localhost/[FRONT CONTROLLER]/about
    http://localhost/[FRONT CONTROLLER]/contacts
    
    
Symfony2 comes with two default front controllers, which are:

.. code:: text

    app.php
    app_dev.php
    
so when your website runs in production environment you would use the app.php front controller:

.. code:: text

    http://localhost/app.php/homepage
    http://localhost/app.php/about
    http://localhost/app.php/contacts
    
if you want to run your site in the development environment you would use the following
addresses:

.. code:: text

    http://localhost/app_dev.php/homepage
    http://localhost/app_dev.php/about
    http://localhost/app_dev.php/contacts
    
    
What changes between using one front controller instead another one? Just the configuration. 
In fact, your application may have as many front controllers as you require and each of them 
has a different configuration that changes the behaviour of your application in
each environment. 

So we can say that "Each front controller defines an environment".

.. note::

    **app.php** is the default front controller and it is the one used when no front
    controller has been explicitly declared. For this reason, you can safety omit it:
    **http://localhost/contacts** will serve the contacts page in production.
    

RedKite CMS Environments
---------------------------

RedKite CMS introduces four other environments to the Symfony2 default ones:

.. code:: text

    rkcms.php
    rkcms_dev.php
    stage.php
    stage_dev.php

.. note::

    Devs environments simply enable the debug for the environment they
    inherit.

These environments have been implemented to completely separate the backend
editor from the production environment.

In fact, all the changes you make in the application backend are not deployed to the production 
environment until you explicitly execute that operation.

Let's explain those environments in detail.

RedKite CMS editor environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    
The **rkcms front controller** handles the environment where the RedKite CMS
lives.  So, to edit your website you would use it as follows:

.. code:: text

    http://localhost/rkcms.php

but that's not enough. In fact you must specify an additional **backend** token in the
address:

.. code:: text

    http://localhost/rkcms.php/backend
    
this is because Symfony2 does not secure an entire environment, so RedKite CMS uses
the **backend** token to tell Symfony2 that all the routes that contain that specific
token, requires an authenticated user to have access granted to the requested resource.

When that url is accessed, RedKite CMS (more specifically Symfony's security layer) 
redirects the application to the login page for the authentication process. 

When the user correctly signs in, RedKite CMS opens the website home page and the 
website can now be managed.

.. note::

    The user name is "admin" and its password is "admin".

RedKite CMS stage environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The stage environment is the place where you can review your website before deploying it
to production: it lives between the backend and the frontend (production) environments.

In addition, this is the place where you can implement and test pages which require retrieving data
from the server: for example if you need to fetch some data from a database and render 
them on a page, you will work in this environment to test your page.

To access the stage environment, simply enter the following url in your browser:

.. code:: text

    http://localhost/stage.php
    
or 

.. code:: text

    http://localhost/stage_dev.php
    
for the stage development environment.

.. note::

    The stage environment is not secured.
    
    
.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
