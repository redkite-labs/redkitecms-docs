Upgrade RedKite CMS to use composer
===================================
RedKite CMS repository has been redesigned to simplify the development. That change
involved the whole cms bundles which have been moved from the application vendor folder
to the src one.

Due to that change RedKite CMS bundles have been removed from composer because they come
with the application itself.

So, how to upgrade RedKite CMS when a new release is available since composer management
has been removed?

This is quite simple because you just need to download the new release from RedKite CMS
website and replce the src/RedKiteLabs folder with the patch just downloaded.

Awesome, but if you want to use composer? That's quite easy too.

Configure RedKite CMS with composer
-----------------------------------

Here's are the steps required to handle RedKite CMS with composer.

1. Remove the **src/RedKiteLabs** folder from the RedKite CMS distribution.
2. Open the composer.json that comes with the application.
3. Add the following packages to composer's **require** section:

.. code-block:: text

    "require": {
        [...]
        "redkite-cms/redkite-cms-bundle": "1.1.*",
        "redkite-cms/installer-bundle": "1.1.*",
        "redkite-labs/modern-business-theme-bundle": "1.1.*",
    	"redkite-labs/bootbusiness-theme-bundle": "1.1.*",
        "redkite-cms/redkite-cms-base-blocks": "1.1.*",
        "redkite-cms/tinymce-block-bundle": "1.1.*"
    },

4. Run the composer's update command:

.. code-block:: text

    php composer.phar update




.. class:: fork-and-edit

Found a typo? Is something not correct in this documentation? `Just fork and edit it!`_

.. _`Just fork and edit it!`: https://github.com/redkite-labs/redkitecms-docs
.. _`Add a new App-Block`: http://redkite-labs.com/add-a-new-block-app-to-redkite-cms
.. _`How to change a content at runtime`: http://redkite-labs.com/how-to-change-a-content-at-runtime
