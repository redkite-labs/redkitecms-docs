Change the theme of your website 
================================

This chapter explains how to change the theme of your website.

Introduction
------------

A RedKite CMS theme is made by templates, a set of twig files which contain a special 
twig function delegated to render the content on the page. 

Each template is made by slots, a place-holder where one or more blocks are stored.
A block represent the content displayed on the page.

When you change the theme, under the hood, RedKite CMS parses all the website's pages 
and changes the templates as requested.

For this chapter I'll change the theme used on redkite-labs.com website with the 
**BootbusinessTheme**, the theme which comes with a fresh install of RedKite CMS.

Mapping the templates
---------------------

The very first step to do is to map the current website theme's templates with the 
templates of the **BootbusinessTheme** theme. To do this, I'll open the Themes panel 
from the top toolbar.

On the left there is the active theme used on the website and on the right there are
the available themes.

I look for the **BootbusinessThemeBundle** then I click on the **Activate** button 
placed under the **BootbusinessTheme** to start this procedure.

A new window opens and the current Theme's templates are listed on the left and
near each template, there is a combo box which contains the new Theme's templates.

.. image:: //bundles/redkitelabswebsite/media/change_theme/img_01.jpg

RedKite CMS automatically selects the templates with the same name, others must be
selected manually.

RedKite Labs website theme is made by two templates: 

1. home
2. internal
    
The first one is automatically mapped because the **BootbusinessTheme** has a template
called with the same name, while I must manually map the **internal** template. I choose 
the one named **empty**, simply choosing it from the combo box.

.. image:: //bundles/redkitelabswebsite/media/change_theme/img_02.jpg

Everything is right now, so I click the **Change** button to start the procedure.


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
.. _`Symfony2 cookbook`: http://symfony.com/doc/current/cookbook/bundles/inheritance.html