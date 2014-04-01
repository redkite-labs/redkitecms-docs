Twitter Bootstrap App-Block
===========================

Twitter Bootstrap App-Block implements several Twitter Bootstrap elements and provides an
editor to manage them with redKite CMS. These elements are:

    - Button
    - Dropdown button
    - Split dropdown button
    - Slider
    - Thumbnail list
    - Label
    - Badge

.. note::

	This bundle is already required by the default theme that comes with a fresh RedKite
	CMS Sandbox.

Installation
------------
To install **Twitter Bootstrap App-Block** add the following entry to your Symfony2 application
**composer.json** file:

.. code-block:: text

    {
        [...]

        "require": {

            [...]        

            "redkite-cms/twitter-bootstrap-bundle": "1.0.*",        
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

    php composer.phar update redkite-cms/twitter-bootstrap-bundle

To have the new block available, don't forget to clear the cache for the **rkcms environment**
as follows:

.. code-block:: text

    app/rkconsole cache:clear --env=rkcms

Usage
-----
This App adds the Twitter section to the Blocks adder menu, which contains the blocks
you may choose from.

.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkite-docs