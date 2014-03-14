RedKite CMS tests suite
==========================

Most parts of RedKite CMS have unit and/or functional and/or integrated tests associated 
with them. 

These tests are written using **PHPUnit** and are required when making contributions to 
RedKite CMS.

You'll always find all the Unit, Integrated and Functional tests in the Tests directory. 
By convention unit tests lives under the **Tests/Unit** folder, integrated ones under the **Tests/Integrated** 
directory and functional are under **Tests/Functional** folder.

To run test suite you need PHPUnit installed on your system. You can find how to install this tool
at http://phpunit.de/manual/current/en/installation.html

Run the tests suite
-------------------

To configure your Symfony2 application to run RedKite CMS test suite you need to configure
the PHPUnit configuration file that comes with the application itself. Usually there is a file called
**phpunit.xml.dist** file placed under the **app** folder.

The best way to add your custom configuration is to copy that file to **phpunit.xml** and 
make your changes to this new file.

We need to change the **bootstrap** parameter, which uses the Symfony2 **bootstrap.php.cache**,
with a new one that comes with RedKite CMS:

.. code:: xml

    <phpunit
        [...]
        bootstrap = "../src/RedKiteLabs/RedKiteCmsBundle/Tests/app_bootstrap.php"

Feel free to give a look to this file, anyway it just configures the **dsn** which points to a
sqlite database created in memory. After that, it loads the standard **bootstrap.php.cache**
file.

.. note::

    This configuration is already present in the RedKite CMS Sandbox.

To run the tests suite, simply run the following command from the top directory of your application:

.. code:: text

    phpunit -c app
    
Test can be run directly from the bundle itself. This is very important to understand because 
RedKite CMS is configured to work with **travis-ci** which is an awesome Continuous Integration 
on-line service.

This means that each time a change is pushed to RedKite CMS repository, the tests suite is executed
at **Travis**, so it is immediately clear if the commit is good or breaks the tests.

For this reason it is important to verify the tests suite in this condition before ask for a
pull request.

Follows these steps to achieve this task:

Clone your RedKite CMS forked repository as follows:

.. code:: text

    git clone [your repository]
    
Download composer 

.. code:: text    

    curl -sS https://getcomposer.org/installer | php

or if you don't have curl:

.. code:: text 

    php -r "eval('?>'.file_get_contents('https://getcomposer.org/installer'));"
    
then run 

.. code:: text

    php composer.phar install --dev

From the top root folder of your cloned repository run:

.. code:: text

    phpunit
    
If everything is correct, ask for your PR.

.. note::

    When RedKite CMS tests suite is executed in this scenario, a basic Symfony2 configuration is
    saved under the **Tests/Functional/app** folder


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
