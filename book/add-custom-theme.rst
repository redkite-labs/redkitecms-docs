Add a custom Theme-App to RedKite CMS
=====================================

This chapter explains how to create a new theme for RedKite CMS.

What is a Theme?
---------------

An RedKite CMS Theme Application could be defined as a collection of twig templates which 
have their own assets like javascripts, stylesheets and images, packaged into a well-known 
structure.

How is a Theme-App structured?
-------------------------

A Theme-App is a standalone Symfony2 bundle. Using this approach has several advantages:

1. It is a Symfony2 Bundle
2. It is reusable in many web sites
3. Assets required by the content are packed into a well known structure

Create the FancyThemeBundle
---------------------------
RedKite CMS has two built-in commands which help to manage a theme:

- **redkitecms:generate:app-theme**
- **redkitecms:generate:templates**

The first command generates a new App-Theme Bundle, the second one generates the templates 
configuration files.

To start a new theme, you must run the **redkitecms:generate:app-theme** command from your console.

This command extends the Symfony's generate:bundle command, so the process should be 
familiar. Run the following command to start:

.. code-block:: text

    php app/console redkitecms:generate:app-theme --env=rkcms

You'll get the following response:

.. code-block:: text

        Welcome to the Symfony2 bundle generator

    [...]

    Bundle namespace:

Answer as follows:

.. code-block:: text

    Bundle namespace: Acme/Theme/FancyThemeBundle

The proposed bundle name **must be changed** to FancyThemeBundle otherwise you might
have issues later:

.. code-block:: text

    Bundle name [AcmeThemeFancyThemeBundle]: FancyThemeBundle

The following options could be left as default values, but you can also change them to fit your needs.

Don't forget to let the command update the AppKernel for you to enable the bundle.

.. note::

    This command does not manipulate the sites routes.

Your first App-Theme has been created, but, wait a moment, why a new command if
the creation procedure is the same of a normal bundle?

Just because this commands adds a new configuration file into the **config** folder that sets
the services to define the theme. See `themes internal configuration`_ to learn more about
this topic.

Add the twig templates
----------------------

When the theme is created, you must start to add your twig templates to the theme bundle.

The **generate:app-theme** command, added a new **Theme** folder under the **Resources/views**
folder of your App-Theme bundle: your templates must be placed inside that folder.

The Theme folder structure
~~~~~~~~~~~~~~~~~~~~~~~~~~
The Theme folder could be basically structured as follows:

.. code-block:: text

	views
            Theme
                home.html.twig
                internal.html.twig

This theme has two templates: the **home** and the **internal**. This structure will 
definitely work, but, instead of that, a real world example would probably look more 
as the following:

.. code-block:: text

    Theme
        base
            base.html.twig
        home.html.twig
        internal.html.twig

a base template is added into a sub-folder. This one should contain the common parts 
of the website's layout, while the other two templates will inherit from the base 
one.

The theme's configuration generated from that structure, consists in two templates
and three slots configuration files, in fact the files saved into the theme's root folder 
become a template file, while a slot file is generated for all the templates, plus one
named **base.xml**. This last one contains the common slots.

Don't worry about the generation process for now, because it is explained in detail 
in the next paragraphs.

You might need to add more separation to templates, so your theme structure could look 
like the following below:

.. code-block:: text

    Theme
        base
            base.html.twig
        support
            template_a.html.twig
            template_b.html.twig
        home.html.twig
        internal.html.twig
        internal_1.html.twig

in this case the home template inherits from the **template_a.html.twig** and 
the others internal templates from the **template_b.html.twig**. The templates inside
the support folder inherit from the **base.html.twig** template.

In this case if the support templates contain repeated slots, these are merged with 
those found into the **base.html.twig** and all of them are saved into the **base.xml** 
configuration file. 

The design
~~~~~~~~~~

RedKite CMS uses **twig** as template engine, so when you have converted the templates 
to html from your design, you must adapt them to twig.

Clean the template
~~~~~~~~~~~~~~~~~~

First of all, templates do not need the header section because it is inherited by the 
base twig template provided by the CMS or from another custom one. 

Please, remove everything that is external to the body tag:

.. code-block:: html

    <!DOCTYPE html>
    <html>
        <head>
            <title></title>
            <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
            <link href="stylesheets/screen.css" media="screen, projection" rel="stylesheet" type="text/css" />
            <link rel="stylesheet" href="stylesheets/960.css" />
        </head>
        <body>
            [ JUST KEEP THIS ]
        </body>
    </html>
	
The twig template
~~~~~~~~~~~~~~~~~
Create a new twig template file called **home.html.twig** under the **Resources/views/Theme** 
folder. Open it and add the following code:

.. code-block:: html+jinja

    {% extends base_template %}

    {% block body %}
    {% endblock %}

The template must extend the template defined by the ThemeEngineBundle's **base_template** 
parameter. This template must have a body **block** where the contents saved from the 
html template you are creating must be placed:

.. code-block:: html+jinja

    {% block body %}
        [ JUST KEEP THIS ]
    {% endblock %}

You can easily change this template just defining a new parameter in your **config.yml**:

.. code-block:: text

	ThemeEngineBundle:
		base_template: MyAwesomeBundle:Theme:my-base.html-twig
		
.. note::

	When you redefine the base template, be sure to redefine all the block sections
	of base template, because RedKite CMS uses them.

The slots
~~~~~~~~~

Now you must identify the slots on the template. The **slot** is the html tag that 
contains the content you want to edit. For example consider the following code:

.. code-block:: html

    <div id="header">
        <div id="logo">
            <a href="#"><img src="images/logo.png" title="Download RedKite CMS" alt="" /></a>
        </div>
    </div>
    [...]

The content to edit is the one contained inside the logo div. This content must be replaced
by a built-in twig function called **renderSlot**:

.. code-block:: html+jinja

    <div id="header">
        <div id="logo">
            {{ renderSlot('logo') }}
        </div>
    </div>
    [...]

This function requires the name of the slot passed as a string as argument.

The id assigned to the slot is not mandatory, so you could call it as you wish, but 
it is best practice to name the slot's id and the slot name in the same way.

Another best practice to follow is to use the **renderSlot** function inside a **div** tag, 
so should avoid something like this:

.. code-block:: html+jinja

    <p id="logo">
        {{ renderSlot('logo') }}
    </p>

.. note::

    Don't throw away the replaced code, it will be used in a while

Prepare your template to be overridden
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

That code is enough to render the contents placed on the slot logo but you must wrap 
the renderSlot function with a block instruction:

.. code-block:: html+jinja

    <div id="header">
        <div id="logo">
            {% block logo %}
            {{ renderSlot('logo') }}
            {% endblock %}
        </div>
    </div>
    [...]

Define the template assets
~~~~~~~~~~~~~~~~~~~~~~~~~~
Each template comes with one or more external assets, like javascript and stylesheet files,
which must be added to the template adapted to work with RedKite CMS.

The base layout used to render each page provides several sections which can be extend in a
template to add extra assets to the page.

There is a `cookbook entry`_ which covers in detail this topic.


.. note:: 

    Following syntaxes have been deprecated:
	
        - BEGIN-EXTERNAL-STYLESHEETS / END-EXTERNAL-STYLESHEETS
        - BEGIN-EXTERNAL-JAVASCRIPTS / END-EXTERNAL-JAVASCRIPTS
        - BEGIN-CMS-STYLESHEETS / END-CMS-STYLESHEETS
        - BEGIN-CMS-JAVASCRIPTS / END-CMS-JAVASCRIPTS

Define the slot attributes
~~~~~~~~~~~~~~~~~~~~~~~~~~

To define the slot's attributes you must add a twig comment below the block declaration
as follows:

.. code-block:: html+jinja

    <div id="header">
        <div id="logo">
            {# BEGIN-SLOT
                name: logo
                repeated: site
                htmlContent: |
                    <img src="/uploads/assets/media/business-website-original-logo.png" title="Progress website logo" alt="Progress website logo" />
            END-SLOT #}
            {% block logo %}
            {{ renderSlot('logo') }}
            {% endblock %}
        </div>
    </div>
    [...]

Let's explain carefully. Each attribute section must start with the **BEGIN-SLOT** 
directive and it must be closed by the **END-SLOT** directive.

Attributes must be written in valid **yml** syntax. Yml requires a perfect indentation, 
so the first line defines the indentation for the other attributes:

.. code-block:: html+jinja

    {# BEGIN-SLOT
        name: logo
          repeated: site
        htmlContent: |
            <img src="/uploads/assets/media/business-website-original-logo.png" title="Progress website logo" alt="Progress website logo" />
    END-SLOT #}

The code above will fail because the second attribute has a wrong indentation. When
this happens, the section is skipped and the service is not instantiated.

The **name** option is mandatory and if it is omitted, RedKite CMS will skip the slot.

Additional optional arguments
------------------------------

In addiction to **name** option, there are some attributes you could define:

1. blockType
2. htmlContent
3. repeated

The blockType option
~~~~~~~~~~~~~~~~~~~~

Defines the block type that RedKite CMS must add for that slot when a new page is added. 
By default, the block type added is **Text**.

The htmlContent option
~~~~~~~~~~~~~~~~~~~~~~

The **htmlContent** option overrides the default content added by the block, so when 
you want to use the default value, simply don't declare this option.

The repeated option
~~~~~~~~~~~~~~~~~~~

Most of the contents displayed on a web page are repeated through the website pages. 
For example the site logo is usually the same for all the site's pages, while a navigation 
menu is the same for a specific language.

The repeated option manages this behaviour and repeats the content for the blocks 
that live on a slot. The possible values for this option are:

1. page (default)
2. language
3. site

When this argument is not declared, a block repeated at page level is added.

None of them is required, but when you don't need to specify any attribute, you must 
be sure to define however this section:

.. code-block:: html+jinja

    {# BEGIN-SLOT
        name: logo
    END-SLOT #}

Create the templates
~~~~~~~~~~~~~~~~~~~~
When your templates are ready, you may run the command which creates the services in 
the DIC:

.. code-block:: text

    redkitecms:generate:templates FancyThemeBundle  --env=rkcms

This command will generate the config files that define the theme's templates and their 
slots. If something goes wrong, a notice is displayed.

Overriding a template
---------------------

To override the template of and existing Theme, you must create a new folder named as 
the theme you want to use, for example **AwesomeThemeBundle**, under the **app/Resources/views** 
folder of your application, than add a new template under that folder, called as the 
one you want to override, for example **home.twig.html**. 

Open that template and add the following code:

.. code-block:: jinja

    // app/Resources/views/AwesomeThemeBundle/home.html.twig
    {% extends 'AwesomeThemeBundle:Theme:home.html.twig' %}

    {% block logo %}
    {{ renderSlot('logo') }}
    {% endblock %}

This code overrides the **AwesomeThemeBundle's home.html.twig** template, replacing the 
**logo** slot with the contents saved in the **logo** slot.

.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
.. _`themes internal configuration`: the-internals-of-theme-configuration
.. _`cookbook entry`: 