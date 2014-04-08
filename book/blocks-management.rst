How to edit blocks
==================

This chapter explains in detail how to manage website contents.

Blocks
------

A **Block** is a container for one kind of content. A block can contain a simple
content like an hypertext, an image, a link but it can also contain more articulated kinds
of content, like a button, a carousel, a thumbnail from Twitter Bootstrap
or the Facebook like button.

A Block comes always with an editor able to handle its content. Usually each editor is
rendered inside a popover placed under the block to edit, except hypertext contents, which
are directly managed on the page, using an inline editor.

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-1.jpg
    :class: img-responsive

Here you can see the editor for a Twitter Bootstrap button.

The rectangle that surrounds the block you are editing, changes to blue (red arrow) but 
the **Block's menu** remains active, so you can always change the editor by just clicking 
on another block (orange arrow).

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-2.jpg
    :class: img-responsive


Interact with blocks
--------------------

To start editing a page, you must click on the **Edit button**, placed on the control 
panel placed on the left side of the page. 

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-14.jpg
    :class: img-responsive

This action turns on the editable blocks, which are highlighted by a red rectangle 
that overlays the active block, when the mouse is placed over it.

On the right bottom corner of this highlighter square, there is a small toolbar
which contains three icons:

1. Plus icon, top arrow adds a new block over the active one
2. Plus icon, bottom arrow adds a new block under the active one
3. Trash icon removes the active block
    
Add a block
^^^^^^^^^^^

To add a new block, you must first highlight it, then you must click on the 
**plus icon top arrow **, pointed by the red arrow on the picture, when you need to
add a block over the highlighted one or on the **plus icon bottom arrow **, pointed by 
the orange arrow on the picture, when you need to add a block under the currently highlighted
block.

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-3.jpg
    :class: img-responsive

A panel, which contains all the available blocks, opens over the button icon. Blocks are listed 
in groups. RedKite CMS comes with several base blocks which are grouped under the
**Default** group, while other listed blocks could have been added by third-parties themes
or could have been required directly from the application

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-4.jpg
    :class: img-responsive

When you add your custom App-Block to RedKite CMS, this is displayed under the 
group it is assigned by the block's developer.

To add a block to the page simply click over it: an Hypertext block has been chosen
in the picture below.

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-5.jpg
    :class: img-responsive


Remove a block
^^^^^^^^^^^^^^

To remove a block you must highlight it and then click on the **trash icon** placed 
into the small toolbar attached to the block highlighter.

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-6.jpg
    :class: img-responsive

You will be prompted to confirm the operation.

Edit a block
^^^^^^^^^^^^

To start editing the content handled by a block, you must place the mouse over it and click into
its area to open the editor.

Usually a RedKite CMS block has an editor rendered into a popover placed under the active 
block, but the default hypertext editor lets you edit blocks inline on the page.

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-7.jpg
    :class: img-responsive

Each editor is different than others, so you may have a form where you can enter the
attributes for an image, you may have a single textarea where to enter a script from
an external website, for example to display an embedded video on your page, you may have a 
list of images and a form to change their attributes as for the Twitter bootstrap carousel.

All popover editors provide a **Save** button placed at the bottom right of the popover 
itself. This button saves the content to the database.  

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-15.jpg
    :class: img-responsive

For hypertext inline editors the save button is placed inside the editor's toolbar.

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-16.jpg
    :class: img-responsive

Please notice that in the example pitcure the save button is not available because
any change to the content had been made.

Sometimes it could happen that all the blocks placed on a slot are removed. In this 
case RedKite CMS adds a place-holder that covers this situation.

This place-holder works as a normal block with the only difference that it is not editable.

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-8.jpg
    :class: img-responsive

Included blocks
^^^^^^^^^^^^^^^

An included block is a block that contains two or more basic blocks. These blocks are 
editable one by one and the including block may be editable too.

A perfect example to explain this kind of block, is the **Rich thumbnails list** App-Block.
It is a gray bordered container that includes an image and a hypertext.

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-9.jpg
    :class: img-responsive

You can edit the image

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-10.jpg
    :class: img-responsive

modify the hypertext to describe the image 

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-11.jpg
    :class: img-responsive

and change the size of the container, editing the container block.

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-12.jpg
    :class: img-responsive

.. note::

    A slot which contains an included block, can accept only a single block. For example 
    you are not allowed to add a Twitter Bootstrap button, or any other kind of blocks 
    under the hypertext included block.
    
List of blocks
^^^^^^^^^^^^^^

A **List of blocks** is a particular block which can contain single and/or included blocks
and renders them in an horizontal or vertical row.

When these blocks are edited, each child block gets two icons placed in the bottom right
corner of the block itself. 

Plus icon adds another block next the one you clicked, trash icon removes the block.

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-13.jpg
    :class: img-responsive

This block is designed to add always the same kind of block when you click on the add 
button.

Despite of that, the Menu Block has another behaviour, in fact, when you click the add 
button, it lets you choose the block you want to add, from the Blocks adder panel.

.. image:: //bundles/redkitelabswebsite/media/manual/contents/contents-17.jpg
    :class: img-responsive


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
