RedKite CMS installation procedure
==================================

RedKite CMS requires a Symfony2 Standard edition application to run. We created several
precompiled distributions of our Content Management System to cover a wide range of 
scenarios:

- `RedKite CMS Sandbox Get & Go`_
- `RedKite CMS Sandbox`_
- `RedKite CMS Sandbox without vendors`_

There is a dedicated page for each distribution where you can get the package and learn 
how to install and configure it.

Sometimes you might need to install RedKite CMS from scratch, for
example when you want to use RedKite CMS with an existing project.

The following paragraphs of this chapter will focus on this topic.

Add RedKite CMS to a Symfony2 project from scratch
------------------------------------------------------
The very first thing to do is to add RedKite CMS to the composer.json file:

.. code-block:: text

    {
        "repositories": [
            {
                "type": "composer",
                "url": "http://apps.redkite-labs.com/"
            }
        ],
        "require": {
            [...]
            "redkite-cms/redkite-cms-bundle": "1.1.*",
            "redkite-cms/installer-bundle": "1.1.*",
            "redkite-labs/bootbusiness-theme-bundle": "1.1.*",
            "redkite-cms/redkite-cms-base-blocks": "1.1.*",
	    "redkite-cms/tinymce-block-bundle": "1.1.*"
        },
        "minimum-stability": "dev"
    }

.. note::

    RedKite CMS uses TinyMCE as the default editor to manage hypertext content, but
    there is a bundle that uses the CKEditor editor. If you are more comfortable with
    this one, replace the **"redkite-cms/tinymce-block-bundle" : "dev-master"**
    declaration with **"redkite-cms/ckeditor-block-bundle" : "dev-master"**

Install RedKite CMS
----------------------

To get the packages, run the following command from a console:

.. code-block:: text

    php composer.phar update


Install other dependencies
--------------------------

As of the 1.1.2 release, RedKite CMS does not require the **yui compressor** package, so you 
can choose the minifier tool you prefer or none.

Learn more about this topic on `Symfony2 website`_ 


The deploy bundle
-----------------

From the Symfony2 book:

    Before you begin, you'll need to create a bundle. Learn more about this topic
    from the `Symfony2 book`_

RedKite CMS is a bundle that extends the Symfony framework.  It is not a "stand alone" bundle and thus it requires 
that you create a bundle.

From the top level folder of your Symfony2 application, run the following command:

.. code-block:: text

    php app/console generate:bundle

When prompted by the generate bundle command you should enter your default company and bundle names. For example 
RedKite Labs website deploy bundle has been called **RedKiteLabs/WebSiteBundle** where **RedKiteLabs**
is the company name and **WebSiteBundle** is the bundle name.

.. note::

    RedKite CMS installer proposes by default **Acme** as company name and **WebSiteBundle** 
    as bundle: just enter yours when you are prompted.


Add the RedKite CMS installer bundle to AppKernel
----------------------------------------------------

To enable the RedKite CMS installer you must add it to your AppKernel file:

.. code-block:: php

    //app/AppKernel.php

    public function registerBundles()
    {
        $bundles = array(

            [...]   
            
            new RedKiteCms\InstallerBundle\RedKiteCmsInstallerBundle(),
        );
    }

Website controller
------------------
Websites routes are handled in production by a single **Controller** which is named **Website**
by default.

You must add this controller to your application to render your website. This task 
is achieved by adding a new controller or by simply modifying the default one added by Symfony. 

Add the file **WebSiteController.php** inside the Controller folder of your bundle.  Then open it 
and add the following code:

.. code-block:: php
    
    namespace Your\Bundle\Controller

    use RedKiteLabs\ThemeEngineBundle\Core\Rendering\Controller\FrontendController;

    class WebSiteController extends FrontendController
    {
    }

.. note::

    Do not forget to change the **namespace** to match your configuration.

If you want to use a controller with a different name, you must rename the
controller itself, then you must tell RedKite CMS to generate the routes pointing to
this controller.

This last step is achieved by adding the following configuration to your **config_rkcms.yml**
file:

.. code-block:: text

    // app/config/config_rkcms.yml

    red_kite_cms:
        deploy_bundle:
          controller: Site

Do not forget to rename the controller to **SiteController.php** and change the controller's 
code as follows:

.. code-block:: php
    
    namespace Your\Bundle\Controller

    use RedKiteLabs\ThemeEngineBundle\Core\Rendering\Controller\FrontendController;

    class SiteController extends FrontendController
    {
    }

Install assets
--------------

RedKite CMS uses Twitter's **bower** package manager to manage the external assets
required by RedKite CMS.

A console command is provided to generate the required **component.json** file in 
the application web folder, which usually is called **web**. Run the following command 
to create that file:

.. code-block:: text

    php app/console redkitecms:build:bower

If you plan to use a different folder, you can specify it as follows:

.. code-block:: text
 
    php app/console redkitecms:build:bower --web-folder=[folder name]

Finally, to install the assets, enter into the application's web folder and run the following
command:

.. code-block:: text

    bower install


.. note::

    if you don't have **bower** installed, you can download the RedKite CMS Sandbox and
    grab the **components** folder from the package's **web** directory, and then copy 
    it into your application's web folder.

Configure the FileBundle
------------------------
FileBundle is a base App-Block that handles a file. This file can be rendered on the page 
as a link to the file itself or it can render its contents.

To get it working you must define the **kernel_root_dir** param under the **twig** section
of the application **config.yml** file:

.. code-block:: text

    twig:
        [...]
        globals:
          kernel_root_dir: %kernel.root_dir%


Remove the AcmeDemoBundle if present
--------------------------------------
Symfony2 comes with a built-in demo bundle which should be removed:

Delete the **src/Acme/DemoBundle** folder.

Delete the following code from **app/AppKernel.php**

.. code-block:: php

    // app/AppKernel.php
    $bundles[] = new Acme\DemoBundle\AcmeDemoBundle();


Delete the following code from **app/config/routing_dev.yml**

.. code-block:: text

    # app/config/routing_dev.yml
    _welcome:
        pattern: /
        defaults: { _controller: AcmeDemoBundle:Welcome:index }

    _demo_secured:
        resource: "@AcmeDemoBundle/Controller/SecuredController.php"
        type: annotation

    _demo:
        resource: "@AcmeDemoBundle/Controller/DemoController.php"
        type: annotation
        prefix: /demo

Clear your cache:

.. code-block:: text

    php app/console cache:clear

Add the installer routes for web interface
------------------------------------------
Finally, if you are going to use the web interface provided by the **RedKiteCmsInstallerBundle**, 
you must add the routes for the install bundle:

.. code-block:: text
    
    // app/config/routing.yml
    _RedKiteCmsInstallerBundle:
        resource: "@RedKiteCmsInstallerBundle/Resources/config/routing.yml"

.. note::

    If you plan to install using the console, you can safety skip this step.

Install
-------
Now you are ready to install RedKite CMS, so follow the instructions provided
for the RedKite CMS Sandbox.


What to do if something goes wrong
----------------------------------
The RedKite CMS installer changes some of the configuration files in your application,
so if something goes wrong during the set-up, you could have problems running the install
process again after these changes have been implemented.

Luckily, the installer backs up those files.  So to fix the problem, you simply have to
remove the files changed by the installer and restore the backed up ones.

Those files are:

.. code-block:: text

    app/AppKernel.php
    app/config/config.yml
    app/config/routing.yml

For each of these files, the installer creates a special copy with the **.bak** extension
before changing the file itself.

If the bak file does not exist, it means that the file has not been changed yet.


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
.. _`composer`: http://getcomposer.org
.. _`Symfony2 setup and configuration tutorial`: http://symfony.com/doc/current/book/installation.html#configuration-and-setup
.. _`yui compressor`: https://github.com/yui/yuicompressor/downloads
.. _`Symfony2 book`: http://symfony.com/doc/current/book/page_creation.html#before-you-begin-create-the-bundle
.. _`RedKite CMS Sandbox Get & Go` : download-get-and-go-redkite-cms-sandbox
.. _`RedKite CMS Sandbox` : download-redkite-cms-sandbox
.. _`RedKite CMS Sandbox without vendors` : download-redkite-cms-sandbox-without-vendors
.. _`Symfony2 website` : http://symfony.com/doc/current/cookbook/assetic/index.html
