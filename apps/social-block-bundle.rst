Social Block App-Block
======================

Social Block App-Block adds social buttons to your website. At the moment the following
buttons are available:

    - Twitter Share button
    - Facebook Like bundle

Installation
------------
To install **Social Block App-Block** add the following entry to your Symfony2 application
**composer.json** file:

.. code-block:: text

    {
        [...]

        "require": {

            [...]        

            "redkite-cms/social-block-bundle": "1.0.*",        
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

    php composer.phar update redkite-cms/social-block-bundle

To have the new block available, don't forget to clear the cache for the **rkcms environment**
as follows:

.. code-block:: text

    app/console cache:clear --env=rkcms

Usage
-----
This App adds the Social section to the Blocks adder menu, which contains the blocks
you may choose from.

Facebook Like
~~~~~~~~~~~~~
The block's editor is tabbed. The first tab contains a form that requires you to define 
the button's attributes, while the second one contains another form where you can define the 
Open Graph options.

To have detailed information about this attributes, please refer the official `facebook`_
documentation.

.. note::

    Open Graph require you to specify the **admins** options: you can retrieve this
    information at `facebook`_, in the **Step 2 - Get Open Graph Tags** section.


Twitter Share
~~~~~~~~~~~~~
The block's editor contains a form which exposes the twitter share button's attributes.

To have detailed information about this attributes, please refer the official `twitter`_
documentation

.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkite-docs
.. _`facebook`: http://developers.facebook.com/docs/reference/plugins/like/
.. _`twitter`: https://twitter.com/about/resources/buttons