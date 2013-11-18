RedKite CMS and Twitter Bootstrap
=================================
RedKite CMS depends on Twitter Bootstrap because of its interface is enhanced by this
awesome framework. 

This does not mean that your website must use Twitter Bootstrap to work with RedKite CMS,
in fact it can handle a theme powered by Twitter Bootstrap 2.x or 3.x versions or 
neither.

Default Twitter Bootstrap version
---------------------------------
By default RedKite CMS uses Twitter Bootstrap 3.x version to render its interface, but it
scales to 2.x when the theme in uses requires this version, so, when a theme does not require 
Twitter Bootstrap, RedKite CMS will still use the 3.x family of this framework.

Require a specific Twitter Bootstrap version
--------------------------------------------
When a theme requires Twitter Bootstrap 2.x, as for the default one that comes with 
RedKite CMS Sandbox, it must explicitly require that version, defining a parameter in 
the configuration file.

To implement this configuration, you must create a **config.yml** file inside the
**Resources/config** folder of your theme bundle and then add the following configuration:


.. code-block:: text

    red_kite_labs_theme_engine:
        bootstrap:
            theme: [{theme: [ThemeBundle Name], version: 2.x}]

replacing the **[ThemeBundle Name]** with the name of your theme bundle. For your convenience
follows the configuration used by the **BootbusinessThemeBundle**:

.. code-block:: text

    red_kite_labs_theme_engine:
        bootstrap:
            theme: [{theme: BootbusinessThemeBundle, version: 2.x}]

The **ThemeEngineBundle** provides the **bootstrap option** which requires the **theme**
param to be implemented.

This last one must be defined as an array with two keys:

- **theme**: The theme bundle to handle
- **version**: The Twitter Bootstrap version to use. This option supports the 2.x value
for Twitter Bootstrap 2 family and 3.x value for 3 family.

A theme powered by Twitter Bootstrap 3
--------------------------------------
Actually RedKite CMS uses the 3.x family as default. If your theme uses the same family
you might skip to implement **the bootstrap parameter** for your theme.

While that works, it is strongly suggested to explicitly configure that parameter
to avoid unwanted behaviors when Twitter Bootstrap will be updated.

For your convenience follows the RedKite labs website theme configuration:

.. code-block:: text
    red_kite_labs_theme_engine:
      bootstrap:
        theme: [{theme: RedKiteLabsThemeBundle, version: 3.x}]

.. class:: fork-and-edit

Found a typo? Is something not correct in this documentation? `Just fork and edit it!`_

.. _`Just fork and edit it!`: https://github.com/redkite-labs/redkitecms-docs
