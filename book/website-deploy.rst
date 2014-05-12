Website deploying
=================

This chapter explains in detail how to deploy your website from the RedKite CMS
backend editor to the stage or production environments.

Deploy the website
------------------

The RedKite CMS website deploying process consists in converting the data entered in the
backend, to Symfony2 templates and routes. And it consists in copying assets from the backend to 
the stage or to the production environments.

The RedKite CMS generates a Twig template for each page you entered in the backend. This
Twig template inherits from the assigned template and it creates the Twig blocks from the 
App-Blocks on the page. These blocks override the ones defined in the original template.

Each page generates a Symfony2 route to map the url (permalink) with the Symfony2
controller which renders the page.

At last, it removes the current assets and replaces them, by copying of all the new assets 
from the backend to the stage or to the production folders.

All these files are generated and copied to the **Deploy bundle** defined when 
RedKite CMS has been installed.

.. note::

    The deploying process is very quick but it takes some time because, when the
    generation process ends, RedKite CMS must call the Symfony2 commands 
    to install and dump assets and to clear the cache.


Preliminary configuration
^^^^^^^^^^^^^^^^^^^^^^^^^
RedKite CMS is highly decoupled from your Symfony2 application and, since 1.1.3
release, it lies on its own kernel. This means you need to configure manually
your **app/AppKernel.php** file to get your website working in production.

In particular you need to configure your theme.

Quick setup
^^^^^^^^^^^

The quickest way to configure your theme is by using the **BootstrapBundle**, which
provides the bundles autoloading functionalities. This bundle is already available because
it is used by the RedKite CMS.

Add the following configuration to AppKernel to configure that bundle:

.. code-block:: php

    // app/AppKernel.php
    public function registerBundles()
    {
        $bundles = array(
            [...]

            new RedKiteLabs\RedKiteCms\BootstrapBundle\RedKiteLabsBootstrapBundle(),
        );


        [...]

        $bootstrapper = new \RedKiteLabs\BootstrapBundle\Core\Autoloader\BundlesAutoloader(__DIR__, $this->getEnvironment(), $bundles);
        $bundles = $bootstrapper->getBundles();

        return $bundles;
    }

then replace the  **registerContainerConfiguration** with this one:

.. code-block:: php

    // app/AppKernel.php
    public function registerContainerConfiguration(LoaderInterface $loader)
    {
        $configFolder = __DIR__ . '/config/bundles/config/' . $this->getEnvironment();
        if (is_dir($configFolder)) {
            $finder = new \Symfony\Component\Finder\Finder();
            $configFiles = $finder->depth(0)->name('*.yml')->in($configFolder);
            foreach ($configFiles as $config) {
                $loader->load((string)$config);
            };
        };

        $loader->load(__DIR__.'/config/config_'.$this->getEnvironment().'.yml');
    }


Please note that this change is permanent, so it works for all bundles saved under the **src/RedkiteCms/** namespace. This means
that if you will change your theme, you do not need to repeat this configuration again.

Adopting this approach, leverages you to deal with theme's configuration files. For example if your theme requires **assetic**,
you will configure a **config.yml** file under the theme itself and it will be handled by the **BootstrapBundle**.

On the other hand, that bundle will load all the themes under the **src/RedkiteCms/Themes**, also the ones you don't use in
production.

The deploying process
^^^^^^^^^^^^^^^^^^^^^

Deploying the website is a hard work, but only for the RedKite CMS. In fact
you are only required to click on the environment you'd like to deploy.

To deploy for the stage environment simply click the **Deploy stage** button
from the top toolbar.

To deploy for the production environment simply click the **Deploy prod** button
from the top toolbar.

Both of them will prompt a confirmation message.

The deploying generation result
-------------------------------

When you are deploying for the stage environment, the RedKite CMS generates the 
following folders and files into the deploy bundle:

.. code-block:: text

    Resources
        config
            site_routing_stage.yml
        public
            stage
                css
                js
                files
                media
        view
            RedKiteStage
                [language [n]]
                    base
                        base_template_1.html.twig                        
                        [...]
                        base_template_[n].html.twig
                    page_1.html.twig
                    [...]
                    page_[n].html.twig

.. note::
    
    Files generated for stage environment should be removed when the website goes
	to production.
    
                 
When you are deploying for the production environment, RedKite CMS generates the 
following folders and files into the deploy bundle:

.. code-block:: text

    Resources
        config
            site_routing.yml
        public
            css
            js
            files
            media
        view
            RedKite
                [language[n]]
                    base
                        base_template_1.html.twig                        
                        [...]
                        base_template_[n].html.twig
                    page_1.html.twig
                    [...]
                    page_[n].html.twig



Working in locale
-----------------

If you have installed RedKite CMS directly on your remote server your changes
are immediately displayed on your website, after deploying.

If you manage your website on your laptop, you must transfer files to the remote 
server after you deployed the website.

The RedKite CMS does not provide any tool to do this job. So you can refer to this
Symfony2 cookbook entry which covers the topic in detail.


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
