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

Mapping the templates
---------------------

The very first step to do is to map the current website theme's templates with the 
templates of the new theme. This operation is made from the **Themes panel**.

On the left there is the active theme used on the website and on the right there are
the available themes.

To change a theme click on the **Activate** button placed under the new theme.

The panel which let you map the theme's templates will open.

.. image:: //bundles/redkitelabswebsite/media/manual/themes/themes-2.jpg
    :class: img-responsive

RedKite CMS parses the templates used in the current website and displays them in 
rows. On the right of each template, it places a dropdown where lists all the templates
found in the new theme. 

When RedKite CMS founds templates with the same name, it automatically maps them, while
others must be mapped by hand. 

To map a template manually, simply choose the right template from the dropdown.

.. image:: //bundles/redkitelabswebsite/media/manual/themes/themes-3.jpg
    :class: img-responsive

When all the templates are mapped, click on the **Change** button to start the procedure.


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
.. _`Symfony2 cookbook`: http://symfony.com/doc/current/cookbook/bundles/inheritance.html