Source Code
===========

**RedKite CMS is hosted at GitHub** and uses **Git** for source control. In order to obtain 
the source code, you must first install Git on your system. Instructions for installing 
and setting up Git can be found at http://help.github.com/set-up-git-redirect.

**RedKite CMS** is a full-stacked Symfony2 application enhanced by several bundles which
implement the cms itself. These bundles live under the top level application **src** folder
and are saved under the **RedKiteLabs** and **RedKiteCms** namespaces.

RedKite CMS namespaces
----------------------

The **RedKiteLabs** namespace contains the main bundles while **RedKiteCms** namespace is 
reserved to blocks and themes:

.. code:: text

	src
		RedKiteCms
			Block
				BootBusinessBlocksBundle [Additional blocks for BootBusinessThemeBundle]
				RedKiteCmsBaseBlockBundle [RedKiteCms base blocks]
				TinyMceBlockBundle [TinyMce editor for RedKite CMS]
				TwitterBootstrapBlockBundle [Several blocks to manage twitter Bootstrap elements]
			Theme
				BootBusinessThemeBundle [A full theme for Twitter Bootstrap 2.x]
				ModernBusinessThemeBundle  [A full theme for Twitter Bootstrap 3.x]
		RedKiteLabs
			RedKiteCms
				BootstrapBundle [The bundle designated to autoload other bundles]
				ElFinderBundle [The ElFinder file manager porting]
				InstallerBundle [The RedKite CMS installer]
				RedKiteCmsBundle [The RedKite CMS core bundle]
			ThemeEngineBundle [The RedKite CMS theme engine bundle]
			
All these bundles must be changed from the RedKiteCms repository and not from the bundle,
because all those bundles are saved into a read-only repository and are auto updated whenever
a push is made to main RedKite CMS folder.

Work with the code
------------------

If you want to create a local copy of the source to play with, you can clone 
the main repository using this command:

.. code:: text

    git clone https://github.com/redkite-labs/RedKiteCms.git

while if you're planning on contributing to RedKite CMS, then it's a good idea to fork the 
repository. You can find instructions for forking a repository at http://help.github.com/fork-a-repo/.

After forking the RedKite CMS repository, you need to configure the application's
**composer.json** to use your forked repository:

.. code:: text

    {
        "repositories": [
            {
                "type": "vcs",
                "url": "[ YOUR GITHUB REDKITE CMS FORKED REPO URL]"
            }
        ],
    }
	


Code standards
--------------

RedKite CMS ecosystem php code is written following the `Symfony2 code standards`_.

RedKite CMS follows `Best Practices for structuring Bundles`_ with some
small exceptions:

- Third-party client-side vendor libraries could be added only for App-Blocks
- Test must live under the Unit, Functional, Integrated folders based on their scope, under 
the Tests directory

RedKite CMS Bundle directory structure
--------------------------------------

The RedKiteCmsBundle is the core bundle behind the RedKite CMS, so, due to its importance, we 
describe here its directory and file structure:

.. code:: text

    Command [RedKite CMS CLI commands]
    Controller [Symfony2 controllers which manages RedKite CMS actions]
    Core [RedKite CMS core. Here are saved all the objects used by the cms itself]
    DependencyInjection [The extension that loads RedKite CMS services into the container]
    Model [Propel's auto-generated classes]
    Resources
        config [Configuration files]
        dbupdate [Database patches]
            mysql [ysql specific patches]
        docs [RedKite CMS docs]
        public
            css
                skins
                    bootstrap [Default RedKite CMS skin]
                        lib [Skin sass files]
        skeleton [Twig skeleton files for generators]
        translations [RedKite CMS translation files]
        views [RedKite CMS twig templates]
    Tests [Test suite]
        Functional [Functional tests]
        Integrated [Integrated tests]
        Unit [Unit tests]
    Twig [Twig extensions]


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
.. _`Symfony2 code standards`: http://symfony.com/doc/current/contributing/code/standards.html
.. _`Best Practices for structuring Bundles`: http://symfony.com/doc/current/cookbook/bundles/best_practices.html