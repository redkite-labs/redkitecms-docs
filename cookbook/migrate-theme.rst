Migrate a theme from RedKite CMS Release Candidate
==================================================
This document describes in detail how to migrate a theme written for RedKite CMS 1.1.0
Release Candidate or before to RedKite CMS 1.1.0 Stable.

Preliminary operations
----------------------
Before starting, check that your theme does not contain dirty code. See example below:
	
.. code-block:: html+jinja

    <ul>
        {# <li id="facebook-like-button">
            {% block facebook_like_button %}
                {# BEGIN-SLOT
                    name: facebook_like_button
                    repeated: site
                    htmlContent: |
                        Facebook like
                END-SLOT }
                {{ renderSlot('facebook_like_button') }}
            {% endblock %}
        </li> #}
        <li>...
    </ul>
	
In this example there is a commented slot block which must be removed.

Be sure your slot blocks are declared encapsulating them into a block statement, because
the migration parser looks for blocks and the new theme design requires that encapsulation.

There was an error in the manual which told that slots attributes had to be declared
outside the block statement:

.. code-block:: html+jinja

    {# BEGIN-SLOT
        name: logo
        repeated: site
        htmlContent: |
            <a href="#"><img src="images/logo.png" title="Download RedKite CMS" alt="" /></a>
    END-SLOT #}
    {% block logo %}
        {{ renderSlot('logo') }}
    {% endblock %}
	
That code is wrong and must be replace with this one:
	
.. code-block:: html+jinja

    {% block logo %}
        {# BEGIN-SLOT
            name: logo
            repeated: site
            htmlContent: |
                <a href="#"><img src="images/logo.png" title="Download RedKite CMS" alt="" /></a>
        END-SLOT #}
        {{ renderSlot('logo') }}
    {% endblock %}

When you are done, you must backup your theme because the migration command will rewrite
several parts of your theme, so you must save a backup if something goes wrong.

Migrate the theme
-----------------

To migrate the theme, just run the following command:

.. code-block:: text

    php app/console --env=rkcms redkitecms:migrate:theme [ YourThemeBundle ]

When the operation is completed, open the xml file that defines the theme service,
located at **[ YourThemeBundle ]/Resources/config/your_theme_bundle_theme.xml** file 
and add the following service to the services section:

.. code-block:: xml

    <services>
        <service id="[ your_theme_bundle ]_theme.theme_slots" class="%red_kite_labs_theme_engine.theme_slots.class%">
            <tag name="red_kite_labs_theme.slots" />
        </service>

        [...]
    </services>

This new service must be passes ad second argument to the theme constructor, defined
as a service in this same file:

.. code-block:: xml

    <services>
        <service id="[ your_theme_bundle ]_theme.theme_slots" class="%red_kite_labs_theme_engine.theme_slots.class%">
            <tag name="red_kite_labs_theme.slots" />
        </service>

        <service id="[ your_theme_bundle ]_theme.theme" class="%red_kite_labs_theme_engine.theme.class%">
            <argument type="string">[ YourThemeBundle ]</argument>
            <argument type="service" id="[ your_theme_bundle ]_theme.theme_slots" />
            <tag name="red_kite_labs_theme_engine.themes.theme" />
        </service>
    </services>
	
At last you must rebuild the templates and clear the cache as usual:

.. code-block:: text

    php app/console --env=rkcms redkitecms:generate:templates [ YourThemeBundle ]
    php app/console --env=rkcms ca:c


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs