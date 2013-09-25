RedKite CMS installation procedure
==================================

RedKite CMS is distributed using `composer`_, so the first thing to do is to grab
it from their website.

.. note::

    This tutorial explains how to install RedKite CMS into an existing project,
    in which the dependencies are managed by composer.

    To start an RedKite CMS project from scratch, you are encouraged to use the
    `RedKite CMS sandbox`_.


Composer installation
---------------------

To install composer, move to the root folder of your application then run the following
command to download composer:

.. code-block:: text

    curl -s http://getcomposer.org/installer | php

If you do not have curl installed, then you can use:

.. code-block:: text
	
	 php -r "eval('?>'.file_get_contents('https://getcomposer.org/installer'));"


Add RedKite CMS to your composer.json file
-----------------------------------------

RedKite CMS requires a **Symfony 2.3** application, so download the latest Symfony2 
release then open the composer.json file and add the following packages:

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
            "redkite-cms/redkite-cms-bundle": "dev-master",
			"redkite-cms/installer-bundle": "dev-master",
			"redkite-labs/bootbusiness-theme-bundle": "dev-master",
			"redkite-cms/tinymce-block-bundle": "dev-master"
        },
        "minimum-stability": "dev"
    }

.. note::

    RedKite CMS uses TinyMCE as default editor to manage an hypertext content, but
    there is a bundle that uses the CKEditor editor. If you are more comfortable with
    this one, replace the **"redkite-cms/tinymce-block-bundle" : "dev-master"**
    declaration with **"redkite-cms/ckeditor-block-bundle" : "dev-master"**

Install RedKite CMS
----------------------

To install, run the following command

.. code-block:: text

    php composer.phar update


RedKite CMS set-up
------------------

RedKite CMS requires several steps in order to properly set-up the CMS itself. Luckily
the **RedKiteCmsInstallerBundle** will do the most of the job for you, providing a web 
installer interface or an interactive Symfony2 command to install RedKite CMS.


Install other dependencies
--------------------------

RedKite CMS requires by default the `yui compressor`_ which is useful to compact 
your assets into one, to reduces page time loading. Grab and unpack it into the **app/Resources/java**
folder and rename it **yuicompressor.jar**.

.. note::

    The compiled yui compressor is saved into the package's **build** folder.

    The **java** folder does not exist and must be created.


While it's strongly suggested to use this tool, you may not have it installed. In this case
you must add the following configuration to your **config_rkcms.yml** file:

.. code-block:: text

    app/config/config_rkcms.yml

    red_kite_cms:
        enable_yui_compressor: false

.. note::

    The **config_rkcms.yml** is created into the app/config folder by the RedKite CMS
    installer, so you must install the CMS, then add the configuration as shown.

The deploy bundle
-----------------

From the Symfony2 book:

    Before you begin, you'll need to create a bundle. Learn more about this topic
    from the `Symfony2 book`_

RedKite CMS does not add anything new to Symfony2, so also requires that you create 
that bundle.

From the top level folder of your Symfony2 application, run the following command:

.. code-block:: text

    php app/console generate:bundle

By default RedKite CMS looks for the **Acme/WebSite** bundle. Obviously you can
choose any name you wish for your bundle: the RedKite CMS installer will ask you
for this.

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
Websites routes are handled in production by a single **Controller** named by default
**Website**.

You must add this controller to your application to render your website. This task 
is achieved adding a new controller or simply modify the default one added by Symfony. 

Add a **WebSiteController.php** file inside the Controller folder of your bundle. Open it 
and add this code:

.. code-block:: php
    
    namespace Your\Bundle\Controller

    use RedKiteLabs\ThemeEngineBundle\Core\Rendering\Controller\FrontendController;

    class WebSiteController extends FrontendController
    {
    }

.. note::

    Don't forget to arrange the **namespace** according with your configuration.

If you want to use a controller with a different name, you must obviously rename the
controller itself, then you must tell RedKite CMS to generate the routes pointing
this controller.

This last step is achieved adding the following configuration to your **config_rkcms.yml**
file:

.. code-block:: text

    // app/config/config_rkcms.yml

    red_kite_cms:
        deploy_bundle:
          controller: Site

Don't forget to rename the controller to **SiteController.php** and change the controller's 
code as follows:

.. code-block:: php
    
    namespace Your\Bundle\Controller

    use RedKiteLabs\ThemeEngineBundle\Core\Rendering\Controller\FrontendController;

    class SiteController extends FrontendController
    {
    }

Install assets
--------------

RedKite CMS uses Twitter's **bower** package manager to manage external assets
required by RedKite CMS.

A console command is provided to generate the required **component.json** file under 
the application web folder, which usually is called **web**. Run the following command 
to create that file:

.. code-block:: text

    php app/console redkitecms:build:bower

If you plan to use a different folder, you can specify that one as follows:

.. code-block:: text
 
    php app/console redkitecms:build:bower --web-folder=[folder name]

To finally install the assets, enter into the application's web folder and run the following
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

To have it working you must define the **kernel_root_dir** param under the **twig** section
of the application **config.yml** file:

.. code-block:: text

    twig:
        [...]
        globals:
          kernel_root_dir: %kernel.root_dir%


Remove the AcmeDemoBundle
-------------------------
Symfony2 comes with a built-in demo which should be removed:

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

Xdebug configuration
--------------------
When you use **Xdebug** with your php installation, you need to configure that module
as follows:

.. code-block:: text
    
    xdebug.max_nesting_level=1000

otherwise you could get an error or, worse, the page rendering could stop, without
displaying nothing on the screen.

.. note::

    If you don't use **Xdebug** you can safety skip this step.

Installing from the console
---------------------------

Installing RedKite CMS from the console is really easy:

.. code-block:: text

    app/console redkitecms:install

This will run the interactive command which installs RedKite CMS, so just provide 
the required information and you are done!

Point your browser at

.. code-block:: text

    http://localhost/rkcms.php/backend/en/index

to start using RedKite CMS.

Installing using the web interface
----------------------------------

To start RedKite CMS installation, simply point your browser at:

.. code-block:: text

    http://localhost/app_dev.php/install

Provide the required information and you are done! Once the process is complete, a web
page is rendered with the process summary and gives you the information required
to start.

Permissions
-----------
Don't forget to set-up the permissions on the installation folder as explained in the
`Symfony2 set-up and configuration tutorial`_


What to do if something goes wrong
----------------------------------
The RedKite CMS installer changes some of the configuration files of your application,
so if something goes wrong during the set-up, you could have problems running the install
process again after these changes have been implemented.

Luckily, the installer backs up those files, so to fix the problem, you have simply to
remove the files changed by the installer and restore the backed up ones.

Those files are:

.. code-block:: text

    app/AppKernel.php
    app/config/config.yml
    app/config/routing.yml

For all of those files, the installer creates a specular copy with the **.bak** extension
before changing the file itself.

If the bak file does not exist, it means that the file has not been changed yet.


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
.. _`composer`: http://getcomposer.org
.. _`RedKite CMS sandbox`: download-alphalemon-cms-for-symfony2-framework
.. _`Symfony2 setup and configuration tutorial`: http://symfony.com/doc/current/book/installation.html#configuration-and-setup
.. _`yui compressor`: https://github.com/yui/yuicompressor/downloads
.. _`Symfony2 book`: http://symfony.com/doc/current/book/page_creation.html#before-you-begin-create-the-bundle