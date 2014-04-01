Add the Modern Business Theme-Block to RedKite CMS
==================================================

A great theme design for a professional website build on top of twitter bootstrap 3.x. 
This is the default theme that comes with a Standard RedKite CMS installation.

The original theme can be found at `http://startbootstrap.com/modern-business`_.

Installation
------------

You can add the **Modern Business Theme-Block** to the application managed by RedKite 
CMS, adding it to the **composer.json** of your Symfony2 application:

.. code-block:: text

    {
        [...]
		
        "require": {
            [...]        
            "redkite-labs/modern-business-theme-bundle": "1.1.*",        
        }
    }

Be sure that there is declared the reference to **http://apps.redkite-labs.com** repository,
under the **repositories** option:

.. code-block:: text

    "repositories": [

        [...]

        {
            "type": "composer",
            "url": "http://apps.redkite-labs.com"
        }
    ],

then run the composer's update command:

.. code-block:: text

    php composer.phar update redkite-labs/modern-business-theme-bundle

To have the new block available, don't forget to clear the cache for the **rkcms environment**
as follows:

.. code-block:: text

    app/rkconsole cache:clear --env=rkcms

Theme preview
-------------

To preview the theme, click the **Themes** button on the toolbar, then the **Preview**
button.

.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkite-docs
.. _`http://startbootstrap.com/modern-business`: http://startbootstrap.com/modern-business