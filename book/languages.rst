Manage website languages
========================

This chapter explains in detail how to manage website languages.


The panel interface
-------------------
Website languages are managed using the **Languages panel**: click the **Languages** 
button on the main toolbar placed at the top of the page to open it.

.. image:: //bundles/redkitelabswebsite/media/manual/languages/languages-1.jpg
    :class: img-responsive

The languages panel is divided in two columns: on the left are listed all the languages
used in the website, while on the right it is placed the interface to add or edit
a language.


Add a new language
------------------
To add a new language, simply choose the one you want to add to your website from 
the **Language** combobox, then click the  **Save** button.

.. image:: //bundles/redkitelabswebsite/media/manual/languages/languages-2.jpg
    :class: img-responsive

If you want that the new language will become the website main language check the 
**Is main** checkbox. 

.. image:: //bundles/redkitelabswebsite/media/manual/languages/languages-3.jpg
    :class: img-responsive

The website’s main language is the first language displayed to a visitor of your website.
He does not require to select any specific language. This means that when the http://redkite-labs.com 
address is requested, the language used is the one marked as the main language.

When you add a new language, RedKite CMS copies all the main language contents 
to the new language, generates a temporary permalink for each page, prefixing it with 
the new language name, and updates the linked permalinks for the new language's contents.


Select and de-select a language
-------------------------------

To select a language just click on the language name from the website languages list. 
This will highlight the language and fill up the form with the language's values. 

To deselect a language, just click on the selected one.

Edit a language
---------------

To edit a language you must first select it from the languages list, then you can 
choose a new language from the **Language** combobox.

When you want to promote a language as the website main language, you must select 
the language you want to promote and edit it, checking on the **Is main** checkbox. 

You cannot degrade the current main language, this means you cannot uncheck the **Is Main**
checkbox for the main language.

Click the **Save** button to confirm your changes.

Delete a language
-----------------

To delete a language, just click on the thrash icon placed on the right of the language 
you want to remove.

You will be prompted to confirm the operation.

The main language cannot be removed from the website, so, when you need to remove it
you must first promote another language as main.


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
