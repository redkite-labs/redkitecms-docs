Create the File App-Block
=========================

This chapter explains how to create a block to handle a file which can be rendered on 
a page as a link or its content rendered directly on the page. 

In this chapter you will learn:

    1. How to add an App-Block to an existing bundle
    2. How to add a controller and a route to handle the implemented actions
    3. How to add an App-Block's extra asset to RedKite CMS 
    4. How to execute a javascript when the editor popover is opened

.. note::

    This chapter requires as pre-requisite the reading of the `Add a new App-Block`_
    chapter
    
Add an App-Block to an existing bundle
--------------------------------------

An App-Block can live as a stand alone bundle or it can be packed together with other
App-Blocks in one bundle.

Just to explain how to add a block to an existing bundle we will add this new App-Block 
to the **BootstrapButtonTutorialBlockBundle**, though these two blocks does not tie together
so much.

The App-Block class
~~~~~~~~~~~~~~~~~~~

To add the new App-Block class to the BootstrapButtonTutorialBlockBundle, just create 
the **AlBlockManagerFileTutorial.php** file inside the **BootstrapButtonTutorialBlockBundle/Core/Block**, 
open it and add the following code:

.. code-block:: php   

    // src/RedKiteCms/Block/BootstrapButtonTutorialBlockBundle/Core/Block/AlBlockManagerFileTutorial.php  
    namespace RedKiteCms\Block\BootstrapButtonTutorialBlockBundle\Core\Block;

    use RedKiteLabs\RedKiteCmsBundle\Core\Content\Block\JsonBlock\AlBlockManagerJsonBlockContainer;

    class AlBlockManagerFileTutorial extends AlBlockManagerJsonBlockContainer
    {
        public function getDefaultValue()
        {
            $value = 
            '{
                "0" : {
                    "file" : "Click to load a file",
                    "description" : "",
                    "opened" : false
                }
            }';

            return array(
                'Content' => $value,
            );
        }
    }
    
This block has three attributes handled by a json markup which are declared in the 
**getDefaultValue** method.

The first step to take is to render the file as link, so we must override the default 
**renderHtml** method as follows:

.. code-block:: php   

    [...]
    use RedKiteLabs\RedKiteCmsBundle\Core\AssetsPath\AlAssetsPath;
 
    protected function renderHtml()
    {
        $items = $this->decodeJsonContent($this->alBlock);
        $item = $items[0];
        $file = $item['file'];
        
        $options = array(
            'webfolder' => $this->container->getParameter('red_kite_cms.web_folder'),
            'folder' => AlAssetsPath::getUploadFolder($this->container),
            'filename' => $file,
        );
        
        $options['displayValue'] = (array_key_exists('description', $item) && ! empty($item['description'])) ? $item['description'] : $file;
                
        return array('RenderView' => array(
            'view' => 'BootstrapButtonTutorialBlockBundle:Content:File/file.html.twig',
            'options' => $options,
        ));
    }

The **AlAssetsPath** provides the paths for common assets folders, in our block we
need the upload folder, so we will use the **getUploadFolder** method which returns
this information.

This method renders the **BootstrapButtonTutorialBlockBundle:File:file.html.twig**
which has not been created yet, so add the **file.html.twig** under the BootstrapButtonTutorialBlockBundle's
**Resources/views/File** folder, open it and add the following code:

.. code-block:: jinja

    {% extends "RedKiteCmsBundle:Block:Editor/_editor.html.twig" %}

    {% block body %}
    <a href="/{{ folder }}/{{ filename }}" {{ editor|raw }}>{{ displayValue }}</a>
    {% endblock %}
    
At last we must define the parameters passed to the block's editor:
    
.. code-block:: php   
    
    public function editorParameters()
    {
        $items = $this->decodeJsonContent($this->alBlock);
        $item = $items[0];
             
        $formClass = $this->container->get('file.form');
        $form = $this->container->get('form.factory')->create($formClass, $item); 
        
        return array(
            'template' => 'RedKiteCmsBundle:Block:Editor/_editor_form.html.twig',
            'title' => Files editor,
            'form' => $form->createView(),
        );
    }

We don't need to define a new editor template because we will use the **_editor_form.html.twig**
provided by RedKiteCms.

.. note ::

    For simplicity, the editor uses the **file.form** service, declared in the **RedKiteCmsBaseBlocksBundle**,
    which defined the editor's form: feel free to give it a look.


Declare the App-Block as a service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To have to App-Bock working, we must open the **app_block.xml** and add the App-Block class as a service:

.. code-block:: xml

    // src/RedKiteCms/Block/BootstrapButtonTutorialBlockBundle/Resources/config/app_block.xml
    <parameters>
        [...]
        <parameter key="bootstrap_file_tutorial_block.block.class">RedKiteCms\Block\BootstrapButtonTutorialBlockBundle\Core\Block\AlBlockManagerFileTutorial</parameter>
    </parameters>

    <services>    
        [...]    
        <service id="bootstrap_file_tutorial_block.block" class="%bootstrap_file_tutorial_block.block.class%">
            <tag name="red_kite_cms.blocks_factory.block" description="File Tutorial" type="FileTutorialBlock" group="bootstrap,Twitter Bootstrap" />
            <argument type="service" id="service_container" />
        </service>
    </services>
    
Load a file
-----------

To load or update the file we must use the Media Library that comes with RedKite CMS.

To accomplish this task we must add a javascript action which must take care to open
the media library and add the reference to the chosen file to the file input box.

The media library requires a connection to an action where it must be defined a connector
to bind the media library itself with the server, so we would have to add a new controller
with a new route.

The controller
~~~~~~~~~~~~~~

To add the controller just create the new **ElFinderFileTutorialController.php** class file
under the **Controller** folder, open it and add the following code:

.. code-block:: php   
    
    // src/RedKiteCms/Block/BootstrapButtonTutorialBlockBundle/Controller
    namespace RedKiteCms\Block\BootstrapButtonTutorialBlockBundle\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\Controller;

    class ElFinderFileTutorialController extends Controller
    {
        public function connectFileAction()
        {
            $connector = $this->container->get('el_finder.file_tutorial_connector');
            $connector->connect();
        }
    }

This action is really simple, it gets the **el_finder.file_tutorial_connector** service, we are
creating in the next paragraph, and calls the **connect** method. We don't need to
return a Response here because the connect method takes care to return the right
data for us.

The route for the controller
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To run that action we must create a new route, so create the **file.xml** file under the
**Resources/config/routing/file**, open it and add the following code inside:

.. code-block:: xml  

    // src/RedKiteCms/Block/BootstrapButtonTutorialBlockBundle/Resources/config/routing/file
    <?xml version="1.0" encoding="UTF-8" ?>

    <routes xmlns="http://symfony.com/schema/routing"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://symfony.com/schema/routing http://symfony.com/schema/routing/routing-1.0.xsd">

        <route id="_file_connect" pattern="/backend/{_locale}/al_elFinderFileTutorialConnect">
            <default key="_controller">BootstrapButtonTutorialBlockBundle:ElFinderFileTutorial:connectFile</default>
            <default key="_locale">en</default>
        </route>
    </routes>

Now we must create a **routing.yml** file under the **Resources/config** folder, open it
and add the following code:

.. code-block:: yml

    _bootstrap_button_tutorial_block_file:
        resource: "@BootstrapButtonTutorialBlockBundle/Resources/config/routing/file/file.xml"

When you add a routing.yml file under a bundle managed my the **RedKiteLabsBootstrapBundle**
it takes care to autoload the routes for you.


The ElFinderFileConnector service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now we must create the ElFinder service, so add a new **ElFinderFileTutorialConnector.php**
class under the **Core/ElFinder/File** folder, open it and add the following code:

.. code-block:: php

    // src/RedKiteCms/Block/BootstrapButtonTutorialBlockBundle/Core/ElFinder/File
    namespace RedKiteCms\Block\BootstrapButtonTutorialBlockBundle\Core\ElFinder\File;

    use RedKiteLabs\RedKiteCmsBundle\Core\ElFinder\Base\ElFinderBaseConnector;

    class ElFinderFileTutorialConnector extends ElFinderBaseConnector
    {
        protected function configure()
        {
            return $this->generateOptions('files', 'Files');
        }
    }

This object inherits from a base connector object, you should give a look, and defines
the mandatory **configure** method which returns an array of options, generated by the
**generateOptions** method, which requires a folder name as first argument and an alias
for the second one.

.. note::

    For simplicity the folder name has been hardcoded, in the real world it is
    declared as a container's parameter.


This service must be declared in the Dependency Injector Container as follows:

.. code-block:: xml

    // src/RedKiteCms/Block/BootstrapButtonTutorialBlockBundle/Resources/config/app_block.xml
    <parameters>
        [...]
        <parameter key="el_finder.file_tutorial_connector">RedKiteCms\Block\BootstrapButtonTutorialBlockBundle\Core\ElFinder\File\ElFinderFileTutorialConnector</parameter>        
    </parameters>

    <services>    
        [...]    
        <service id="el_finder.file_tutorial_connector" class="%el_finder.file_tutorial_connector%" >
            <argument type="service" id="service_container" />
        </service>
    </services>

The javascript asset
~~~~~~~~~~~~~~~~~~~~

Everything is ready, so we just need to add a javascript asset which will take care 
to open the media library.

Create a new **file_tutorial_editor.js** under the **Resources/public/file/js** folder,
open it and add the following code:

.. code-block:: js

    // src/RedKiteCms/Block/BootstrapButtonTutorialBlockBundle/Resources/public/file/js/file_tutorial_editor.js
    $(document).ready(function() {
        $(document).on("popoverShow", function(event, element){
            if (element.attr('data-type') != 'FileTutorialBlock') {
                return;
            }

            // your code
        });
    }); 

This code responds to the **popoverShow** event triggered when the editor popover 
is opened. This event passes as second argument the element which is being edited,
so we must check that it belongs the block type we are working on, in this example
the **FileTutorial**.
    

Now we will add the code to open the media library under the [ your code ] section:

.. code-block:: js

    $('#al_json_block_file').click(function()
    {              
        $('<div/>').dialogelfinder({
            url : frontController + 'backend/' + $('#al_available_languages option:selected').val() + '/al_elFinderFileTutorialConnect',
            lang : 'en',
            width : 840,
            destroyOnClose : true,
            commandsOptions : {
                getfile: {
                    oncomplete: 'destroy'
                }
            },
            getFileCallback : function(file, fm) {
                $('#al_json_block_file').val(file.path);
            }
        }).dialogelfinder('instance');
    });

First of all, we bind the **al_json_block_file event click**, so each time the users
click into the file inputbox, the media library is opened.

The most important options to point out are the **url** which executes the action we implemented
before and the **getFileCallback** which sets back the file path.

Add the asset to the cms
~~~~~~~~~~~~~~~~~~~~~~~~

The last thing to do is to add this asset to the cms. This task is made adding a parameter
to the DIC, so open the app_block.xml file and add the following code inside:

.. code-block:: xml

    // src/RedKiteCms/Block/BootstrapButtonTutorialBlockBundle/Resources/config/app_block.xml
    <parameters>
        [...]
        <parameter key="file.external_javascripts.cms" type="collection">
            <parameter>@BootstrapButtonTutorialBlockBundle/Resources/public/file/js/file_tutorial_editor.js</parameter>
        </parameter>
    </parameters>  

This parameter is parsed by RedKite CMS and added to the external javascripts only when 
the editor is active to avoid loading this asset in production.

This last task is made adding the **cms** suffix to the **file.external_javascripts**
key.

.. note ::

    To have your asset available you must run the 

  
Use your App-Block
------------------

To use your new App-Block, enter inside RedKite CMS backend and just add it to your 
website from the adder blocks menu.
  
Conclusion
----------

After reading this chapter you should be able to add an App-Block to an existing bundle,
add a controller and a route to handle the implemented actions, add an App-Block's extra 
asset to RedKite CMS, override the default action to save the block's content

.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/alphalemon/alphalemon-docs
.. _`Add a new App-Block`: http://www.alphalemon.com/add-a-new-block-app-to-alphalemon-cms