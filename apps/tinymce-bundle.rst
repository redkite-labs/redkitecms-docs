TinyMCE Editor App-Block
========================

The TinyMCE Editor App-Block implements the awesome TinyMCE editor to manage html website
contents inline on the web page.

.. note::

	This bundle is the default html contents editor and it already comes with the 
	RedKite CMS Sandbox.

Installation
------------
To install **TinyMCE Editor App-Block** add the following entry to your Symfony2 application
**composer.json** file:

.. code-block:: text

    {
        [...]

        "require": {

            [...]        

            "redkite-cms/tinymce-block-bundle": "1.0.*",        
        }
    }

Be sure to have a reference to **http://apps.redkite-labs.com** repository, under
under the composer **repositories** option:

.. code-block:: text

    "repositories": [

        [...]

        {
            "type": "composer",
            "url": "http://apps.redkite-labs.com/"
        }
    ],

then run the composer's update command:

.. code-block:: text

    php composer.phar update redkite-cms/tinymce-bundle

To have the new block available, don't forget to clear the cache for the **rkcms environment**
as follows:

.. code-block:: text

    app/console cache:clear --env=rkcms
	
.. note::

	To use this bundle be sure to have any other bundles that manage Html contents,
	like the CkEditor, added to your application.

Usage
-----
This App adds the **Hypertext (TinyMCE)** entry under the Default section of the Blocks 
adder menu.

.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkite-docs