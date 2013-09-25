Website deploying
=================

This chapter explains in detail how to deploy your website from the RedKite CMS
backend editor, to the stage or production environments.

Deploy the website
------------------

RedKite CMS website deploying process consist in converting the data entered in the
backend, to Symfony2 templates and routes and to copy assets from the backend to 
stage or production environments.

RedKite CMS generates a Twig template for each page you entered in the backend. This
template inherits from the assigned template and creates the Twig blocks from the 
App-Blocks on the page. These blocks override the ones defined in the original template.

Each page generates a Symfony2 route to map the url (permalink) with the Symfony2
controller which renders the page.

At last, it removes the current assets and replaces them, copying all the new assets 
from the backend to stage or production folders.

All these files are generated and copied to the **Deploy bundle** defined when 
RedKite CMS has been installed.

.. note::

    The deploying process is very quick but it takes some time because, when the
    generation process ends, RedKite CMS must call the Symfony2 commands 
    to install and dump assets and to clear the cache.


The deploying process
^^^^^^^^^^^^^^^^^^^^^

Deploying the website is a hard work, but only for RedKite CMS, in fact
you are only required to click on the environment you'd like to deploy.

To deploy for the stage environment simply click the **Deploy stage** button
from the top toolbar.

To deploy for the production environment simply click the **Deploy prod** button
from the top toolbar.

Both of them will prompt a confirmation message.

The deploying generation result
-------------------------------

When you deploying for the stage environment, RedKite CMS generates the 
following folders and files into the deploy bundle:

.. code:: text

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
    
                 
When you deploying for the production environment, RedKite CMS generates the 
following folders and files into the deploy bundle:

.. code:: text

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

RedKite CMS does not provide any tool to do this job, so you can refer this
Symfony2 cookbook entry which covers the topic in detail.


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs