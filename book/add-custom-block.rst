Add a custom block to RedKite CMS
=================================
This chapter explains how to create a custom block and use it with RedKite CMS. 
In detail it explains how the create, the **TutorialLinkBlock** to manage a link.

In this chapter you will learn:

    1. How to create a new block using the built-in command
    2. How to create a Block to describe the content to render
    3. How to create the BlockManager to handle the block and how to add it to the DIC
    4. How to create a twig template to render the block
    5. How to create a twig template to render based a a form to handle the block properties
    6. How to create configure the templates as parameters in the DIC


What is a Block
---------------
A **Block** is a container for one kind of content. In addiction an **block**
includes the editor to manage the block itself by RedKite CMS, when the backend editor 
is active.

Create the TutorialLinkBlock
----------------------------

RedKite CMS provides a command to generate the additional files required by a block.

Run the following command from your console to generate a new block bundle named
**TutorialLinkBlock** that will live under the **src** folder:

.. code-block:: text

    php app/rkconsole redkitecms:generate:block --env=rkcms

.. note::

    This command must be run under the rkcms environment.

It will start the standard Symfony2 bundle generator command but will ask for additional
information.

Naming conventions
~~~~~~~~~~~~~~~~~~

A block should always be created using a well defined namespace:

.. code-block:: text

    RedKite/Block/[Bundle Name]Bundle

When you follow this convention, you get two advantages:

1. The block is distributable by composer
2. The block is auto loaded without changing anything

If you prefer not follow this conventions, you must run that command with the
**--no-strict** option, then you must change the BundlesAutoloader instantiation 
in your **AppKernel** file as follows:

.. code-block:: php

    // app/AppKernel.php
    public function registerBundles()
    {
        [...]

        $bootstrapper = new \RedKiteLabs\BootstrapBundle\Core\Autoloader\BundlesAutoloader(__DIR__, $this->getEnvironment(), $bundles, null, array([path 1], [path 2]));
    }

replacing the **[path n]** entries with the paths of your blocks.

Let's suppose you want to add a new block under the **src/Acme/Blocks/MyAwesomeBlockBundle**
folder of your application. The path to pass to the **BundlesAutoloader** is the following:

.. code-block:: php
    
    __DIR__ . '/../src/Acme/Blocks'

Block generation
~~~~~~~~~~~~~~~~

Let's see the bundle generation in detail.

.. code-block:: text

    Welcome to the RedKite CMS block bundle generator
    [...]

    Bundle namespace:

Enter the bundle name, as follows:

.. code-block:: text

    Bundle namespace: RedKiteCms/Block/TutorialLinkBlock

The proposed name is fine:

.. code-block:: text

    Bundle name [RedKiteCmsBlockTutorialLinkBundle]:

The standard directory is fine:

.. code-block:: text

    The bundle can be generated anywhere. The suggested default directory uses
    the standard conventions.

    Target directory [/home/RedKite/www/RedKiteCmsSandbox/src]:


To help you get started faster, the command can generate some
code snippets for you.

.. code-block:: text

    Do you want to generate the whole directory structure [no]? 


Now you are asked for the App-Block description, which is the one displayed in the
contextual menu used to add a block to page:

.. code-block:: text

    Please enter the description that identifies your App-Block content.
    The value you enter will be displayed in the adding menu.

    Block description: Tutorial link

Then you are asked for the Block group. Blocks that belongs the same group
are kept together in the block adding menu.

.. code-block:: text

    Please enter the group name to keep together the App-Blocks that belongs that group.

    App-Block group: tutorial,Tutorial

Well done! Your very first block bundle has been created! The Block just created can be immediately
used but you must clear the cache first for the **rkcms environment**:

.. code-block:: text

    php app/rkconsole ca:c --env=rkcms


How is structured a Block
-------------------------
A Block is defined by a flat php object which exposes the properties needed to manage the content
we want to display on the page.

The first step to create this object, is to try to describe the content looking for one or more properties 
required to build that content on the page. In our specific case, we can say that a link can be defined by 
two properties: 

	- An href property which will handle the page where the link will navigate when it is clicked.
	- A value property which will handle the displayed value on the page.

Our block has just been defined so we can write the flat php class that describes it:

.. code-block:: php

    // src/RedKiteCms/Block/TutorialLinkBundle/Core/Block/TutorialLinkBlock.php
	namespace RedKiteCms\Block\TutorialLinkBundle\Core\Block;

	use JMS\Serializer\Annotation\Type;
	use RedKiteLabs\RedKiteCms\RedKiteCmsBundle\Core\Content\Block\BaseBlock;

	class TutorialLinkBlock extends BaseBlock
	{
		/**
		 * @Type("string")
		 */
		protected $type = "TutorialLink";
		/**
		 * @Type("string")
		 */
		protected $href = "#";
		/**
		 * @Type("string")
		 */
		protected $value = "This is a link";

		public function setHref($v)
		{
			$this->href = $v;
		}

		public function getHref()
		{
			return $this->href;
		}

		public function setValue($v)
		{
			$this->value = $v;
		}

		public function getValue()
		{
			return $this->value;
		}
	}

A block object must always inherit from the abstract **RedKiteLabs\RedKiteCms\RedKiteCmsBundle\Core\Content\Block\BaseBlock**
class, which defines some base properties every block will have.

This class requires to define the type property, which handles the type of block
you are building, in our example the block will handle a **TutorialLink** object.
	
Our object defines the **href** and **value** properties, which are the two specific properties
we found to describe a link. They are declared as **protected** properties and declares the default value
used when the object is instantiated. We also defined the getters and setters to handle them from the outside.

As last you should have noticed that all the properties have defined their type as a 
**Serializer's annotation**, this because this object is serialized and de-serialized by 
the **JMSSerializerBundle** to be saved into the database.

The block generator has created the **TutorialLinkBlock.php** file: just open it an replace the contents
with the above code.


The BlockManager object
~~~~~~~~~~~~~~~~~~~~~~~
Now that our block class is ready, we must define the **BlockManager** object which will handle the block
class just create:

.. code-block:: php

    // src/RedKiteCms/Block/TutorialLinkBundle/Core/Block/BlockManagerTutorialLink.php
	namespace RedKiteCms\Block\TutorialLinkBundle\Core\Block;

	use RedKiteLabs\RedKiteCms\RedKiteCmsBundle\Core\Content\Block\BlockManager;

	class BlockManagerTutorialLink extends BlockManager
	{

		/**
		 * Defines the block's default value
		 *
		 * @return array
		 */
		public function getBlockClass()
		{
			return 'RedKiteCms\Block\TutorialLinkBundle\Core\Block\TutorialLinkBlock';
		}
	}
	
Our BlockManager must inherit from the **RedKiteLabs\RedKiteCms\RedKiteCmsBundle\Core\Content\Block\BlockManager**
object, an abstract object that implements the **RedKiteLabs\RedKiteCms\RedKiteCmsBundle\Core\Content\ContentManagerInterface**
and the **RedKiteLabs\RedKiteCms\RedKiteCmsBundle\Core\Content\Block\BlockManagerInterface** which define the methods
to manage a block.

This class requires you to implement the **getBlockClass** method which must return the block's class namespace handled
by the block manager itself.

The block generator has created the **BlockManagerTutorialLink.php** file which is already fine for our block.
The generator added some extra commented code to highlight some useful methods you could need when you develop
your block: you should take some time to give them a look.


Add the BlockManager to the DIC
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The **BlockManagerTutorialLink** just created must be added to the Symfony2 Dependency Injector Container and it
must be tagged as **red_kite_cms.blocks_factory.block**. In detail it must be added to the **app_block.xml**
configuration file, which handles the RedKite CMS services for a block bundle.

The block generator added the following code to the **app_block.xml** configuration file for you:

.. code-block:: xml

	<!-- src/RedKiteCms/Block/TutorialLinkBundle/Resources/config/app_block.xml -->
	<parameters>
        <parameter key="tutorial_link.block.class">RedKiteCms\Block\TutorialLinkBundle\Core\Block\BlockManagerTutorialLink</parameter>
    </parameters>

    <services>
        <service id="tutorial_link.block" class="%tutorial_link.block.class%">
            <argument type="service" id="serializer" />
            <argument type="service" id="red_kite_cms.events_handler" />
            <argument type="service" id="red_kite_cms.factory_repository" />
            <argument type="service" id="red_kite_cms.block_factory" />
            <tag name="red_kite_cms.blocks_factory.block" description="Tutorial link" type="TutorialLink" group="tutorial,Tutorial" />
        </service>
    </services>
	
A BlockManager object depends on several services passed as arguments as you can notice from the service declaration,
but this is trivial. The most important part of this service is how it is tagged. Let's go deeper into the tag's properties.

The **name** property must always be **red_kite_cms.blocks_factory.block*+ to define a BlockManager object.
The **type** property is the BlockManager type used by RedKite CMS to identify a block.
The **description** property is how the block appears into the **adder menu**.
The **group** property groups the blocks in the **adder menu** by the first argument and uses the second to title the group.
	

The block and editor templates
------------------------------
Our block requires a twig template to define the html design of the content we want to create, and  
it will require a template for the editor required to manage the block.


The block template
~~~~~~~~~~~~~~~~~~
To properly render the block on the page we must add the following template:

.. code-block:: jinja

    {# src/RedKiteCms/Block/TutorialLinkBundle/Resources/views/Content/tutoriallink.html.twig #}
	{% block body %}
		<a href="{{ block.getHref }}" {% if block.getClass is defined %}class="{{ block.getClass }}"{% endif %} {{ renderEditor(block, forceEmptyEditor) }}>{{ block.getValue }}</a>
	{% endblock %}

This template will always receive the **block** variable which handles the Block object derived from the **BaseBlock** class, 
in our example the **TutorialLinkBlock**, so it is quite easy to render the get the object properties, accessing them by 
their getters.

At last, we must render the block's editor, simply calling the **renderEditor(block, forceEmptyEditor)**. This function
requires a **BaseBlock** object as first argument and, as second argument, a boolean value which returns an empty editor 
when it is true.

.. note::

	When you build your template you can add this function exactly as proposed here, because both arguments are passed to 
	the template form the twig function responsible to render the block.

The block generator has already generated that file, so open it and replace the generated code with the one above.


The editor template
-------------------
The TutorialLinkBlock will require an editor, so we must define the template that renders it. The editor will expose
two inputbox which will handle the TutorialLinkBlock's href and value properties and it will be displayed into a 
Bootstrap pop-over.


The form
~~~~~~~~
To handle the block properties we will use a Symfony2 form, which is defined as follows:

.. code-block:: php

    // src/RedKiteCms/Block/TutorialLinkBundle/Core/Form/TutorialLinkType.php
	namespace RedKiteCms\Block\TutorialLinkBundle\Core\Form;

	use RedKiteLabs\RedKiteCms\RedKiteCmsBaseBlocksBundle\Core\Form\Base\BaseType;
	use Symfony\Component\Form\FormBuilderInterface;

	class TutorialLinkType extends BaseType
	{
		public function buildForm(FormBuilderInterface $builder, array $options)
		{
			$builder->add('href');
			$builder->add('value');
			$this->addClassAttribute($builder);

			parent::buildForm($builder, $options);
		}
	}
	
The form class inherits from the **RedKiteLabs\RedKiteCms\RedKiteCmsBaseBlocksBundle\Core\Form\Base\BaseType**
which handles the block common properties to handle.

Here we have defined the **href** and **value** fields as two inputbox to handle the TutorialLinkBlock's properties. 
In addition we have added an extra inputbox to handle the linkclass, by calling the **addClassAttribute** method.

The block generator has already generated that file, so open it and replace the generated code with the one above.


The editor
~~~~~~~~~~
Our editor will be simply defined as follows:

.. code-block:: jinja

    {# src/RedKiteCms/Block/TutorialLinkBundle/Resources/views/Editor/tutoriallink.html.twig #}
	{{ include("RedKiteCmsBundle:Block:Editor/_editor_form.html.twig") }}
	
so the template that renders an editor, provided by RedKite CMS, will be rendered.

The block generator has already generated that file.


The block and editor templates configuration
--------------------------------------------
The block template, the editor and the form must be declared as parameters in the block's **config_rkcms.yml** file, as follows:

.. code-block:: text

    # src/RedKiteCms/Block/TutorialLinkBundle/Resources/config/config_rkcms.yml
    red_kite_cms_block_tutorial_link:
      tutoriallink:
        block_template: 'RedKiteCmsBlockTutorialLinkBundle:Content:tutorial_link.html.twig'
        editor_template: 'RedKiteCmsBlockTutorialLinkBundle:Editor:tutorial_link.html.twig'
        editor_handler: 'RedKiteCms\Block\TutorialLinkBundle\Core\Form\TutorialLinkType'
	
In detail, the block configuration must be declared for the bundle extension alias, **red_kite_cms_block_tutorial_link** in
our example.

Under that configuration parameter you must list the blocks which must be declared as the block type in lowercase, **tutoriallink**
in our example.

Each block can handle the following parameters:

	- block_template (Mandatory) 	
	- editor_template
	- editor_handler

The **block_template** is the only mandatory parameter and defines the template which will render the content
block on the page.

The **editor_template** defines the template which will render the block's editor.

The **editor_handler** defines an handler for the editor. This is usually a Symfony2 form.

The block generator has already generated that file.


Add the block configuration to the container
--------------------------------------------
The configuration just created must be added to the Symfony2 container, adding it to the Bundle's **Configuration** and
**DependencyInjection** classes.


The configuration file
~~~~~~~~~~~~~~~~~~~~~~

The **Configuration** class will be defined as follows:

.. code-block:: php

    // src/RedKiteCms/Block/TutorialLinkBundle/DependencyInjection/Configuration.php
	namespace RedKiteCms\Block\TutorialLinkBundle\DependencyInjection;

	use RedKiteLabs\RedKiteCms\RedKiteCmsBundle\DependencyInjection\BaseBlockConfiguration;
	use Symfony\Component\Config\Definition\Builder\ArrayNodeDefinition;
	use Symfony\Component\Config\Definition\Builder\TreeBuilder;
	use Symfony\Component\Config\Definition\ConfigurationInterface;

	class Configuration extends BaseBlockConfiguration
	{
		/**
		 * {@inheritdoc}
		 */
		public function getConfigTreeBuilder()
		{
			$treeBuilder = new TreeBuilder();
            $rootNode = $treeBuilder->root('red_kite_cms_block_tutorial_link');

            // Here you should define the parameters that are allowed to
            // configure your bundle. See the documentation linked above for
            // more information on that topic.
            $rootNode
                ->addDefaultsIfNotSet()
            ;

            $this->addBlockDefinition($rootNode, 'tutoriallink');

            return $treeBuilder;
		}
	}
	
This class will extend the **RedKiteLabs\RedKiteCms\RedKiteCmsBundle\DependencyInjection\BaseBlockConfiguration**
instead of the usual Symfony2 Configuration class.

This object defines the **addBlockDefinition** method which requires the **rootNode** as first argument and the block's
type in lowercase as second argument, as defined in the **src/RedKiteCms/Block/TutorialLinkBundle/Resources/config/config_rkcms.yml**:

.. code-block:: php
	
	public function getConfigTreeBuilder()
	{
		[...]

		$this->addBlockDefinition($rootNode, 'tutoriallink');

		return $treeBuilder;
	}

The block generator has already generated that file.


The extension file
~~~~~~~~~~~~~~~~~~

The **RedKiteCmsBlockTutorialLinkExtension** class will be defined as follows:

.. code-block:: php

    // src/RedKiteCms/Block/TutorialLinkBundle/DependencyInjection/RedKiteCmsBlockTutorialLinkExtension.php
	namespace RedKiteCms\Block\TutorialLinkBundle\DependencyInjection;

	use RedKiteLabs\RedKiteCms\RedKiteCmsBundle\DependencyInjection\BaseBlockExtension;
	use Symfony\Component\DependencyInjection\ContainerBuilder;
	use Symfony\Component\Config\FileLocator;
	use Symfony\Component\HttpKernel\DependencyInjection\Extension;
	use Symfony\Component\DependencyInjection\Loader;

	class RedKiteCmsBlockTutorialLinkExtension extends BaseBlockExtension
	{
		/**
		 * {@inheritdoc}
		 */
		public function load(array $configs, ContainerBuilder $container)
		{
			$loader = new Loader\XmlFileLoader($container, new FileLocator(__DIR__.'/../Resources/config'));
			$loader->load('services.xml');

			$configuration = new Configuration();
			$config = $this->processConfiguration($configuration, $configs);

			$this->setBlockParameters($container, $config, 'tutoriallink');
		}

		public function getAlias()
		{
			return 'red_kite_cms_block_tutorial_link';
		}
	}

As we saw for the **Configuration** object this one will not inherit from the standard **Symfony2 Extension**
but from the **BaseBlockExtension** class, which defines the **setBlockParameters** which requires the **ContainerBuilder**
object as first argument, the processed **TreeBuilder** object as second argument and the block's type in lowercase as third
argument, as defined in the **src/RedKiteCms/Block/TutorialLinkBundle/Resources/config/config_rkcms.yml**.

The block generator has already generated that file.


Celebrate
---------
Your new block is now ready and it can be used with RedKite CMS.


Conclusion
----------
After reading this chapter you should be able to create a new block using the built-in 
command, create a new object to manage the content rendered on the page, create a service 
that handles that object, create a template to display the content on the web page and create
an editor to manage the content.


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
