How to edit blocks
==================

This chapter explains in detail how to manage website contents.

Blocks
------

A **Block** is a container for one kind of content. A block can contain a simple
content like an hypertext, an image, a link but it can also contain more articulated kind
of contents, like a button, a carousel, a thumbnail from Twitter Bootstrap
or the Facebook like button.

A Block come always with an editor able to handled its content. Usually each editor is
rendered inside a popover placed under the block to edit, except hypertext contents, which
are directly managed on the page, using an inline editor.

.. image:: //bundles/alphalemonwebsite/media/manual/img-5.jpg

Here you can see the editor for a Twitter Bootstrap button.

The rectangle, that surrounds the block you are editing, changes to blue but the **Blocks menu** 
remains active, so you can always change the editor just clicking on another block.

.. image:: //bundles/alphalemonwebsite/media/manual/img-6.jpg


Interact with blocks
--------------------

To start editing a page, you must click on the **Edit button**, placed on the left. 

This action turns on the editable blocks, which are highlighted by a red rectangle 
that overlays the active block, when the mouse is placed over it.

On the right bottom corner of this highlighter square, there is a small toolbar
which contains three icons:

1. Plus icon adds a new block under the active one
2. Cross icon moves the block
3. Trash icon removes the active block

.. image:: //bundles/alphalemonwebsite/media/manual/img-7.jpg
    
.. note::

    The move function is not implemented yet
    
Add a block
^^^^^^^^^^^

To add a new block, you must first highlight it, then you must click on the **plus icon** 
placed into the small toolbar, attached to the block highlighter.

A panel, which contains all the available blocks, opens over the plus icon. Blocks are listed 
in groups. RedKite CMS comes with several base blocks which are grouped under the
**Default** group, while other listed blocks could have been added by third-parties themes
or could have been required directly from the application

.. image:: //bundles/alphalemonwebsite/media/manual/img-8.jpg

When you add an additional App-Block to RedKite CMS, this is displayed under the 
group it is assigned by the block's developer.

To add a block to the page simply click over it.

.. image:: //bundles/alphalemonwebsite/media/manual/img-9.jpg

Here a new hypertext content block has been added.

Remove a block
^^^^^^^^^^^^^^

To remove a block you must highlight it and then click on the **trash icon** placed 
into the small toolbar attached to the block highlighter.

.. image:: //bundles/alphalemonwebsite/media/manual/img-10.jpg

You will be prompted to confirm the operation.

Edit a block
^^^^^^^^^^^^

To start editing the block's content, you must place the mouse over it and click into
its area to open the editor.

Usually a RedKite CMS block has an editor rendered into a popover placed under the active 
block, but the default hypertext editor let you edit blocks inline on the page.

.. image:: //bundles/alphalemonwebsite/media/manual/img-11.jpg

Each editor is different than the others, so you may have a form where you can enter the
attributes for an image, you may have a single textarea where to enter a script from
an external website, for example to display an embedded video on your page, you may have a 
list of images and a form to change their attributes as for the Twitter bootstrap carousel.

All popover editors provide a **Save** button placed at the bottom right of the popover 
itself. This button saves the content to the database. For hypertext inline editors 
the save button is placed inside the editor's toolbar.

Sometimes it could happen that all the blocks placed on a slot are removed. In this 
case RedKite CMS adds a place-holder that explains this situation.

This place-holder works as a normal block with the only difference that it is not editable.

.. image:: //bundles/alphalemonwebsite/media/manual/img-12.jpg

Included blocks
^^^^^^^^^^^^^^^

An included block is a block that contains two or more basic blocks. These blocks are 
editable one by one and the including block may be editable too.

A perfect example to explain this kind of block, is the **BootbusinessThumbnail** App-Block.
It is a gray bordered container that includes an image and a hypertext.

.. image:: //bundles/alphalemonwebsite/media/manual/img-13.jpg

You can change the image

.. image:: //bundles/alphalemonwebsite/media/manual/img-14.jpg

modify the hypertext to describe the image 

.. image:: //bundles/alphalemonwebsite/media/manual/img-15.jpg

and change the size of the container, editing the container block.

.. image:: //bundles/alphalemonwebsite/media/manual/img-16.jpg

.. note::

    A slot which contains an included block, can accept only a single block. For example 
    you are not allowed to add a Twitter Bootstrap button, or any other kind of blocks 
    under the hypertext included block.
    
List of blocks
^^^^^^^^^^^^^^

A **List of blocks** is a particular block which can contain singles and/or included blocks
and renders them in an horizontal or vertical row.

When these blocks are edited, each child block gets two icons placed in the bottom right
corner of the block itself. 

Plus icon adds another block next the one you clicked, trash icon removes the block.

A perfect example is the **BootbusinessThumbnailsList** which displays one or more
**BootbusinessThumbnail** blocks in an horizontal row.

.. image:: //bundles/alphalemonwebsite/media/manual/img-17.jpg

This block is designed to add always a BootbusinessThumbnail: this means that, when you
click the add button a new thumbnail is added.

Despite of that, the Menu Block has another behaviour, in fact, when you click the add 
button, it lets you choose the block you want to add, from the Blocks adder panel.


Add a new block type to your application
----------------------------------------

Blocks could be added to your application in two ways:

1. Create a custom block
2. Add an existing block in your composer.json file

To create a custom block, you should read the `dedicated tutorial`_, while to add an
existing block to your application using composer, you must follow the instructions 
provided by each third-parties block.


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
.. _`dedicated tutorial` : add-a-new-block-app-to-redkite-cms
