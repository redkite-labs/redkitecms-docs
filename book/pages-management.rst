Pages management
================

This chapter explains in detail how an RedKite CMS page is structured and how to 
entirely manage website pages.

The panel interface
-------------------
Website pages are managed using the **Pages panel** click the **Pages** button on 
the main toolbar placed at the top of the page to open it.

The panel is divided in two columns: the bigger placed on the left contains the interface 
to add and modify a page, while on the right there is a list which contains all the website 
pages.

The panel that contains the form to manage the page information is made by tabs, so 
there are several sections you need to fill up to add or modify a page.

The page tab
------------

The **Page** tab contains the page attributes described below.

.. image:: //bundles/redkitelabswebsite/media/manual/img-28.jpg


The page name option
~~~~~~~~~~~~~~~~~~~~
The **Page name** is an unique internal name that globally identifies the page. This
value is inserted into the text box which has the same name.

This information could be manipulated by RedKite CMS when you enter some particular 
characters like spaces or accented letters.

For example the **contact us** page name is transformed to **contact-us**.

This information is mandatory.


The template name option
~~~~~~~~~~~~~~~~~~~~~~~~
The **Template name** represents the name of the layout the page will get. The template
names come from the theme you are using in your website.

This value must be selected using the combobox which has the same name.

This information is mandatory.


The home page option
~~~~~~~~~~~~~~~~~~~~
The website's home page is the first page displayed when a user, that browses your website,
does not require any specific page, for example http://redkite-labs.com address.

If you want that the page you are adding or editing will become the website home page, 
just check this option.

When a page is to become the homepage of the site, you must select that page and check
the **Home page** checkbox: you cannot degrade the current home page to work 
out that job.

The home page cannot be removed from the website, so, when you need to remove it,
you must first promote another page as home.


The published option
~~~~~~~~~~~~~~~~~~~~
This option is useful in a team where members work on the website at the same time, because
it explicitly forces someone with appropriate rights, to declare the page as 
ready to be published in production.

In fact a page is not published by default, so only the users which have enough privileges
could change this option and declare a page as publishable.

Another situation when this option is useful could be when you are working on a page and 
you want keep it private just because it is not ready to be published yet.


The Seo tab
-----------

The **SEO** tab contains the Search Engines Options attributes described below.

.. image:: //bundles/redkitelabswebsite/media/manual/img-29.jpg


The permalink option
~~~~~~~~~~~~~~~~~~~~

The **permalink** represents a static permanent link that identifies the page outside
the site.

This value is inserted into the textarea which has the same name.

A permalink could be translated for each language of your website, for example, 
you may have a page named **contacts** which may have a permalink **contact-us** 
for the English version and another permalink **contattaci** for the Italian language.

Permalink is the most important information you will provide for each page because it is
the name used from the external to reach it. This means you must carefully choose each
permalink. 

Let's dive into an example.

Think about the http://redkite-labs.com website download page: we could have chosen a 
permalink called **download** and it would have worked fine, but we defined a long description
to define this page: **download-redkite-cms-for-symfony2-and-twitter-bootstrap-frameworks**.

This is not a simply matter of taste, in fact we provide a lot of information about 
our product in that permalink: we specify the **download** keyword, the product name, 
which is obviously **redkite**, we declared that it is a **cms** and that is written 
to work with **Symfony2** and **Twitter Bootstrap** frameworks.

All these information should produce a better result when a search engine indexes that
page. 

Obviously the permalink must be consistent with contents saved in the page, otherwise 
search engines will penalize that page and your site.

This information is manipulated, when needed, as for the **Page name** attribute.

The title option
~~~~~~~~~~~~~~~~
This information represents the page title and it is displayed in the top bar of the
browser. 

It should not contain more than 50 characters and must be consistent with page's contents.

This value is inserted into the textarea which has the same name.

This is another important information you must provide while it is not mandatory,
because search engines give it a great importance for indexing the page, so, as for 
permalink you must very accurate to choose the page title.


The description option
~~~~~~~~~~~~~~~~~~~~~~
This information must provide a small brief description about the arguments exposed
on the page. 

This information is not displayed anywhere, it should not contain more than 255 characters 
and must be consistent with page's contents.

This value is inserted into the textarea which has the same name.

While it is not mandatory as for the title, it is another important information you 
must provide.


The keywords option
~~~~~~~~~~~~~~~~~~~
This information should provide a list of keywords used in the page. This one has
been widely abused in the past, so many search engines ignore it today.


The sitemap tab
---------------

A sitemap is a file which is automatically generated by RedKite CMS each time the 
website is deployed. 

This file helps search engines to correctly parse the pages of your website.

.. image:: //bundles/redkitelabswebsite/media/manual/img-30.jpg

From this tab you can set the sitemap attributes for the page.

To learn more about the information you can provide in this section, read the 
`sitemap protocol`_.

Add a new page
--------------

To add a new page you must be sure that any other page is selected in the pages list 
and that the form is completely blank. This is the situation you get when you open the panel.

Fill all the required information and click the **Save** button to confirm.

Select and de-select a page
---------------------------

To select a page you must choose it from the pages list on the right, clicking on the 
page name. 

After that, you must choose the language from the **Languages** combobox placed over 
the pages list, to load the page's attributes for the chosen language

To deselect a page, just click on it.

Edit a page
-----------

To edit a page you must first select it, then you can change what you need and click on
the **Save** button to confirm your changes. 


Delete a page
-------------

To delete a page you must first select it, then you must click the **Delete**
button placed under the pages list.

When you need to do this operation, you are not required to choose a language
because RedKite CMS removes all the page attributes.


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
.. _`sitemap protocol`: http://www.sitemaps.org/protocol.html
