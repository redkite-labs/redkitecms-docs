RedKite CMS security
====================

This chapter covers in detail the RedKite CMS security layer, explaining how to add, 
edit and remove users and roles, and how to customize the RedKite CMS default security 
policy.

Users and roles definition
--------------------------

RedKite CMS exploits the Symfony2 security layer to secure the backend editor and it
provides a high-level interface to manage users and roles.

A user is someone who can access to the backend, providing a username and a password.

A role defines the rights a user has on the available resources.

A user must have a role and this role grants or denies the access to a particular resource.

.. note::

    While it is not mandatory, you are encouraged to read about security on 
    `Symfony2 documentation`_

Roles
-----

RedKite CMS has three predefined roles, in its default configuration:

1. ROLE_SUPER_ADMIN
2. ROLE_ADMIN
3. ROLE_USER
    
**ROLE_SUPER_ADMIN** and **ROLE_ADMIN** have the same rights and have granted the access
to the whole RedKite CMS resources.

**ROLE_USER** have granted the access to the whole RedKite CMS resources, with the exception of the
website deploy and the users management.

Add a new Role
~~~~~~~~~~~~~~

Click the **Security** button on the top toolbar, to open the security panel

.. image:: //bundles/redkitelabswebsite/media/manual/security/security-1.jpg
    :class: img-responsive

then click the **Roles** button placed beside the **New** button, under the users list.

.. image:: //bundles/redkitelabswebsite/media/manual/security/security-2.jpg
    :class: img-responsive

The website roles are now listed on the left while the interface to add or edit a role
is on the right. To add a new role just fill up the **Role** field when any other role
is selected.

.. note::

    A valid role must start with **ROLE_** prefix
    
Click on the **Save** button to confirm your changes.

Edit a Role
~~~~~~~~~~~

To edit a role just select it by clicking on its name and change the value in the form
on the right.

Delete a Role
~~~~~~~~~~~~~

To delete a role, just click on the thrash icon placed on the right of the role you 
want to remove.


Users management
~~~~~~~~~~~~~~~~

The Users management works exactly as explained for the roles. The only difference is
the number of information required. This number is higher for users than for roles.

.. image:: //bundles/redkitelabswebsite/media/manual/security/security-3.jpg
    :class: img-responsive


Advanced configuration
----------------------

When you add new roles and/or you want to implement a restrictive security policy,
you need to modify manually the RedKite CMS security configuration file.

The default **RedKite CMS security file** is saved into the RedKite CMS 
**Resources/config** folder and it is imported into the **RedKite CMS config_rkcms.yml**, 
file as shown below:

.. code-block:: text

    // RedKiteCmsBundle/config/config_rkcms.yml
    imports:
    [...]
        - { resource: "@RedKiteCmsBundle/Resources/config/security.yml" }

RedKite CMS is highly decoupled, and the security layer is not an exception. 

The implemented configuration impacts only on the RedKite CMS backend because it is specific
for the **rkcms** environments. This means you can implement your own security policy in 
production when you need it, without colliding with the backend.

The security file in detail
---------------------------

Here is a detailed explanation on how the security file is made.


The firewall
~~~~~~~~~~~~

The implemented firewall is quite simple. In fact it secures all the urls which start 
with the **backend** token:

.. code-block:: text

        [...]
        red_kite_cms:
            pattern:    ~/backend
            form_login:
                check_path:  /backend/login_check
                login_path:  /backend/login
            logout:
                path:   /backend/logout
                target: /backend/
            http_basic:


The access control list
~~~~~~~~~~~~~~~~~~~~~~~

The basic access control list secures the **users** area which is granted to all the users 
who have at least the **ROLE_ADMIN** role and both the deploying routes which can be 
browsed only by users who have at least the **ROLE_ADMIN** role.

.. code-block:: text

    access_control:
        - { path: "~/backend/[a-z]+/al_(stage|production)Deploy", role: ROLE_ADMIN }
        - { path: ~/backend/users, roles: ROLE_ADMIN }
        - { path: ~/backend, roles: ROLE_USER }


The role hierarchy
~~~~~~~~~~~~~~~~~~

The last configuration is for the role hierarchy, which is implemented as follows:

.. code-block:: text

    role_hierarchy:
        ROLE_ADMIN:       ROLE_USER
        ROLE_SUPER:ADMIN: [ROLE_USER, ROLE_ADMIN, ROLE_ALLOWED_TO_SWITCH]


How to customize the security.yml file
--------------------------------------

Symfony does not permit to import or configure a security file from another 
configuration file. So the only way to change the implemented rules is to modify 
the **security.yml** file that comes with the RedKite CMS.

Obviously, it is really a bad idea to work on the security file that comes with
**RedKiteCmsBundle** bundle, because if you would upgrade the cms, the changes 
you have made would get lost.

To avoid that, you must copy the RedKite's security file into the application's 
config folder, rename it, for example, to **security_cms.yml**, and change the import 
directive in the config_rkcms.yml:

.. code-block:: text

    // app/config/config_rkcms.yml
    imports:
    [...]
    - { resource: "security_cms.yml" }

Customizing the security for your website
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A real life example could be the following one: you may need to add a new role, 
called **ROLE_PUBLISHER**, to allow granted users to gain this role in order to publish 
the website. This leverages the site administrators from this task.

First of all you must add that role to the website as explained before. Then you must 
change the rule that secures the deploy action as follows:

.. code-block:: text

    access_control:
        - { path: ~/backend/[a-z]+/al_(stage|production)Deploy, role: ROLE_PUBLISHER }
        - { path: ~/backend/users, roles: ROLE_ADMIN }
        - { path: ~/backend, roles: ROLE_USER }

To let that work you must change the role_hierarchy as follows:

.. code-block:: text

    role_hierarchy:
        ROLE_PUBLISHER:         ROLE_USER
        ROLE_ADMIN:             ROLE_PUBLISHER
        ROLE_SUPER_ADMIN:       ROLE_ADMIN

You can learn more about this by reading the `Symfony2 security chapter`_.

Let's now assume that you want to avoid users granted by the **ROLE_USER** role to delete 
contents.

The route that points to this action is the **deleteBlock**, so you must add the new security
rule as follows:

.. code-block:: text

    access_control:
        - { path: ~/backend/[a-z]+/deleteBlock, role: ROLE_PUBLISHER }
        - { path: ~/backend/[a-z]+/al_deploy, role: ROLE_PUBLISHER }
        - { path: ~/backend/users, roles: ROLE_ADMIN }
        - { path: ~/backend, roles: ROLE_USER }


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
.. _`Symfony2 documentation`: http://symfony.com/doc/current/book/security.html
.. _`Symfony2 security chapter`: http://symfony.com/doc/current/book/security.html
