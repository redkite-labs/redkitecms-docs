Add a block collection to RedKite CMS
=====================================
This chapter explains how to create a custom block collection and use it with RedKite CMS. 
A block collection is just a block which handles a collection of other blocks.

In detail it explains how the create, the **TutorialMenuBlock** a block that handles a collection 
of Link blocks.

In this chapter you will learn:

    TODO
    2. How to create a Block to describe the content to render
    3. How to create the BlockManager to handle the block and how to add it to the DIC
    4. How to create a twig template to render the block
    5. How to create a twig template to render based a a form to handle the block properties
    6. How to create configure the templates as parameters in the DIC
	
You are required to read the TODO before keep going with this chapter.


Implement the MenuBlock
-----------------------
The first thing to do is to implement the flat php object to define the Block, so add the
**TutorialMenuBlock.php** under the **src/RedKiteCms/Block/TutorialLinkBundle/Core/Block**
folder and paste this code inside:

.. code-block:: php

    // src/RedKiteCms/Block/TutorialLinkBundle/Core/Block/TutorialMenuBlock.php
	namespace RedKiteCms\Block\TutorialLinkBundle\Core\Block;

	use JMS\Serializer\Annotation\Type;
	use RedKiteLabs\RedKiteCms\RedKiteCmsBundle\Core\Content\Block\BaseBlockCollection;
	use Symfony\Component\Translation\TranslatorInterface;

	class TutorialMenuBlock extends BaseBlockCollection
	{
		/**
		 * @Type("string")
		 */
		protected $type = "TutorialMenu";

		public function __construct(TranslatorInterface $translator = null)
		{
			parent::__construct($translator);

			$link = new TutorialLinkBlock($this->translator);
			$this->addChild($link);

			$link = new TutorialLinkBlock($this->translator);
			$this->addChild($link);
		}
	} 

A collection block object must always inherit from the abstract **RedKiteLabs\RedKiteCms\RedKiteCmsBundle\Core\Content\Block\BaseBlockCollection**
which defines the property delegated to handle our blocks collection and the methods to interact with.

The collection is populated in the object's constructor, where each child block is instantiated and then it is added to
the collection.

In our object we are instantiating two TutorialLinkBlock objects:


.. code-block:: php

	$link = new TutorialLinkBlock($this->translator);
	
and than that block is added to the collection, passing it as first argument of the **addChild** method:


.. code-block:: php

	$this->addChild($link);
	
Sometimes you could need to add one or more properties to a block collection and to edit it as for a
block: you can accomplish this task as you do for a normal block. A real example is the 
**src\RedKiteLabs\RedKiteCms\TwitterBootstrapBundle\Core\Block\Navbar\NavbarBlock** object which implements
the Bootstrap navbar, which collects several kind of blocks and implements some properties to handle, 
for example the navbar position.


The BlockManager object
~~~~~~~~~~~~~~~~~~~~~~~
We must define the BlockManager object which will handle the block class just create. Add the
**BlockManagerTutorialMenu.php** file under the **src/RedKiteCms/Block/TutorialLinkBundle/Core/BlockManager**
then paste this code inside:

.. code-block:: php

	// src/RedKiteCms/Block/TutorialLinkBundle/Core/BlockManager/BlockManagerTutorialMenu.php
	namespace RedKiteCms\Block\TutorialLinkBundle\Core\Block;

	use RedKiteLabs\RedKiteCms\RedKiteCmsBundle\Core\Content\Block\BlockManagerCollection;

	class BlockManagerTutorialMenu extends BlockManagerCollection
	{
		public function getBlockClass()
		{
			return 'RedKiteCms\Block\TutorialLinkBundle\Core\Block\TutorialMenuBlock';
		}
	}
	
Our BlockManager must inherit from the **RedKiteLabs\RedKiteCms\RedKiteCmsBundle\Core\Content\Block\BlockManagerCollection**
object which defines some extra methods to handle the block collection.

This class requires you to implement the **getBlockClass** method which must return the block's class namespace handled
by the block manager itself.


Add the BlockManager to the DIC
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Add the following code to the **app_block.xml** configuration file to add the block manager to the DIC:

.. code-block:: xml

    <!-- src/RedKiteCms/Block/TutorialLinkBundle/Resources/config/app_block.xml -->
	<parameters>
        <parameter key="tutorial_menu.block.class">RedKiteCms\Block\TutorialLinkBundle\Core\Block\BlockManagerTutorialMenu</parameter>
	</parameters>

    <services> 
        <service id="tutorial_menu.block" class="%tutorial_menu.block.class%">
            <argument type="service" id="serializer" />
            <argument type="service" id="red_kite_cms.events_handler" />
            <argument type="service" id="red_kite_cms.factory_repository" />
            <argument type="service" id="red_kite_cms.block_factory" />
            <tag name="red_kite_cms.blocks_factory.block" description="Tutorial Menu" type="TutorialMenu" group="tutorial,Tutorial" />
        </service>
	</services>
	
The block template
------------------
To properly render the block on the page we must add the following template. Add the **tutorialmenu.html.twig**
twig template under the **src/RedKiteCms/Block/TutorialLinkBundle/Resources/views/Content** then
paste this code indide:

.. code-block:: jinja

    {# src/RedKiteCms/Block/TutorialLinkBundle/Resources/views/Content/tutorialmenu.html.twig #}
	{% block body %}
	<ul class="nav nav-pills al-menu-list inline-list" {{ renderEditor(block, forceEmptyEditor) }}>
		{% if block.children|length > 0 %}
			{% for key, item in block.children %}
				<li {%  if (item.getActiveClass is defined and item.getActiveClass != "") %}class="{{ item.getActiveClass }}"{% endif %}>{%  include container.getParameter("red_kite_cms." ~ item.getType|lower ~ ".block_template") with {'block' : item}  %}</li>
			{% endfor %}
		{% else %}
			<li class="al-empty">Any link added</li>
		{% endif %}
	</ul>
	{% endblock %}


TODO [ inline-list ]

This block does not require an editor to work.


The block and editor templates configuration
--------------------------------------------
The block template, the editor and the form must be declared as parameters in the block's
**config_rkcms.yml** file, as follows:

.. code-block:: text

    # src/RedKiteCms/Block/TutorialLinkBundle/Resources/config/config_rkcms.yml
    red_kite_cms_block_tutorial_link:
      [...]
      tutorialmenu:
        block_template: 'RedKiteCmsBlockTutorialLinkBundle:Content:tutorial_menu.html.twig'


Add the block configuration to the container
--------------------------------------------
We need to add the block configuration to the existing **Configuration** class:

.. code-block:: php

    // src/RedKiteCms/Block/TutorialLinkBundle/DependencyInjection/Configuration.php
	class Configuration extends BaseBlockConfiguration
	{
		/**
		 * {@inheritdoc}
		 */
		public function getConfigTreeBuilder()
		{
			[...]

			$this->addBlockDefinition($rootNode, 'tutorialmenu');

			return $treeBuilder;
		}
	}
	
then we need to update the RedKiteCmsBlockTutorialLinkExtension as follows:

.. code-block:: php
    // src/RedKiteCms/Block/TutorialLinkBundle/DependencyInjection/RedKiteCmsBlockTutorialLinkExtension.php
	class RedKiteCmsBlockTutorialLinkExtension extends BaseBlockExtension
	{
		/**
		 * {@inheritdoc}
		 */
		public function load(array $configs, ContainerBuilder $container)
		{
			[...]

			$this->setBlockParameters($container, $config, 'tutorialmenu');
		}
	}
	
	
Add a javascript listener to add children blocks
------------------------------------------------
The block has already been programmed to be an inline list, but we must decide which kind of blocks we
can add to the menu and we must remove the inline list interface when the user stops to edit a block.

This task is accomplished adding two javascript listeners which will respond to the **startEditingBlocks**
and to the **stopEditingBlocks** events.

Add a new **menu_editor.js** file under the **src/RedKiteCms/Block/TutorialLinkBundle/Resources/public/menu/js** and paste the following code inside:

.. code-block:: javascript
    // src/RedKiteCms/Block/TutorialLinkBundle/Resources/public/menu/js/menu_editor.js
	$(document).ready(function() {
		$(document).on("startEditingBlocks", function(event, element){
			if (element.attr('data-type') != 'TutorialMenu') {
				return;
			}
			
			$(element)
				.inlinelist('start', { 'target': 'li > a', 'filterBlocks': 'TutorialLink' })
			;
			
		});
		
		$(document).on("stopEditingBlocks", function(event, element){ 
			if (element.attr('data-type') != 'TutorialMenu') {
				return;
			}
					
			$(element)
				.inlinelist('stop')
				.blocksEditor('start')
				.find('[data-editor="enabled"]')
				.blocksEditor('start')
			;
		});
	});
	
Both of those listeners, are executed only for a **TutorialMenu** type:

.. code-block:: javascript
	
	if (element.attr('data-type') != 'TutorialMenu') {
		return;
	}
	
When the **startEditingBlocks** event is raised, the inline list is started by the following instruction:

.. code-block:: javascript

	$(element)
		.inlinelist('start', { 'target': 'li > a', 'filterBlocks': 'TutorialLink' })
	;

Let's examine the arguments passed to the **inlinelist** method. The first one is the target where the inline
list interface is added. That interface consists in two buttons capable to add or remove the selected child.


Filter blocks for adder menu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The second argument filters the items in the **adder menu** and shows only the ones in the **filterBlocks** list.

When you need to filter more than a block, just separe them by a comma, for example:

.. code-block:: javascript

	$(element)
		.inlinelist('start', { 'target': 'li > a', 'filterBlocks': 'TutorialLink,BootstrapNavbarDropdownBlock' })
	;


Install assets
~~~~~~~~~~~~~~
At last, don't forget you must install the assets to have the javascript loaded, so just run the following command from the
RedKite CMS console:

.. code-block:: text

    php app/rkconsole --env=rkcms assets:install --symlink web

Add a specific block to the inline list
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Sometimes it could be necessary to add a specific block to the list without displaying the adders menu.
A real example for this situation is the case of the **ButtonsGroup block** which is a collection of buttons
so it must contain only that kind of blocks. In that case, the **inlinelist** method must be called
passing the **addValue** switch as argument:

.. code-block:: javascript

     element.inlinelist('start', { 
	  target: 'button',
	  addValue: '{"operation": "add", "value": { "blockType": "BootstrapButtonBlock" }}'
	});

	
Insert an asset into the web page
---------------------------------
The last step is to add the asset we just created to the web page, simply adding it to the DIC, adding
the following parameter to the **app_block.xml** file:

.. code-block:: xml

    <!-- src/RedKiteCms/Block/TutorialLinkBundle/Resources/config/app_block.xml -->
	<parameters>
		[...]
		
		<parameter key="tutorialmenu.external_javascripts.cms" type="collection">
			<parameter>@RedKiteCmsBlockTutorialLinkBundle/Resources/public/js/menu_editor.js</parameter>
		</parameter>
	</parameters>

The most important argument here is the **key** one which must follow this rule:

.. code-block::text

    block_type.asset_type[.cms]

The **block_type** token must be the block type name in lower case, the **asset_type** specifies the kind of asset we want to
define and it could be on eof the following:

    - external_javascripts
    - external_stylesheets
    - internal_javascripts
    - internal_stylesheets

The optional **.cms** adds the asset only when the RedKite CMS editor is active, so it is ignored in production.

Celebrate
---------
Your new collection block is now ready and it can be used with RedKite CMS.
    
Conclusion
----------

After reading this chapter you should be able to create a new block using the built-in 
command, create a new object to manage the content rendered on the page, create a service 
that handles that object, manage a json content instead of an html content, create a 
template to display the content and create an editor to manage the content.


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs