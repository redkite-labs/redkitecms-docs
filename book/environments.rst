Environments
============

This chapter explains in detail RedKite CMS environments configuration. 


.. note::

    This is a quite technical chapter but explains just a simple concept, so you should
    take it easy.


What is a work environment?
---------------------------

Symfony2 handles several environments where your website may run. Here there is the 
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
    
They introduced the concept of **front controller** which is a simple php file that 
defines the unique entry point for the entire application.

Let's explain this concept by examples. Supposing your website has three pages: home, 
about and contacts pages.

To open those pages you will require the following addresses in your browser:

.. code:: text

    http://localhost/[FRONT CONTROLLER]/homepage
    http://localhost/[FRONT CONTROLLER]/about
    http://localhost/[FRONT CONTROLLER]/contacts
    
Symfony2 comes with two default front controllers, which are:

.. code:: text

    app.php
    app_dev.php
    
so when your website runs in production environment you will use the app.php front controller:

.. code:: text

    http://localhost/app.php/homepage
    http://localhost/app.php/about
    http://localhost/app.php/contacts
    
if you want to run your site in the development environment your will require the following
addresses:

.. code:: text

    http://localhost/app_dev.php/homepage
    http://localhost/app_dev.php/about
    http://localhost/app_dev.php/contacts
    
What changes between using one front controller instead another one? Just the configuration. 
In fact, your application may have as many front controllers you require and each of them 
has a different configuration that changes the behaviour of your application that runs in
one particular environment. 

So we can say that "Each front controller defines an environment".

.. note::

    **app.php** is the default front controller and it is the one used when any front
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

These environments have been implemented to keep completely separated the backend
editor from the production environment.

In fact, all the changes you made in the application backend, are not deployed to production 
environment until you explicitly do that operation.

Let's explain those environments in detail.

RedKite CMS editor environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    
The **rkcms front controller** handles the environment where RedKite CMS
lives, so to edit your website you must use it as follows:

.. code:: text

    http://localhost/rkcms.php

but that's not enough. In fact you must specify an additional **backend** token in the
address:

.. code:: text

    http://localhost/rkcms.php/backend
    
this because Symfony2 does not secure an entire environment, so RedKite CMS uses
the **backend** token to tell Symfony2 that all the routes that contain that specific
token, requires an authenticated user to have the access granted to the requested resource.

When that url is required, RedKite CMS, but I should say Symfony2 to be more precise, 
redirects the application to the login page for the authentication process. 

When the user correctly signed in, RedKite CMS opens the website home page and the whole
website can be managed.

RedKite CMS stage environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The stage environment is the place where you can review your website before deploy it
to production: it lives between the backend and the frontend (production) environments.

In addiction, it is the place where you can implement pages which require to fetch data
from the server: for example if you need to fetch some data from a database and render 
them on a page, you will work in this environment to implement your page.

To enter the stage environment, simply enter the following url in your browser:

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
