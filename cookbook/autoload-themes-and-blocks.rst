How to autoload Themes and Blocks
=================================

RedKite CMS comes with **RedKiteLabsBootstrapBundle** a bundle designed to autoload 
and configure Symfony2 bundles for an application which dependencies are managed by 
composer. 

RedKite CMS Themes and Blocks are Symfony2 bundles, so to autoload them without requiring 
to declare it in Symfony2's AppKernel, you just need to create an **autoloader.json** file 
on the bundle's top folder and let the RedKiteLabsBootstrapBundle do the hard job for you.

When you create a new Theme or Block using the provided command, this one creates the 
autoload file for you.

You can autoload your bundles by environments and you can add a base configuration 
to your bundle, saved in a config.yml file which comes with the bundle itself, to define
the base configuration for your bundle the user should manually add to application's
config.yml file.

As for configuration, you can define your routes in the routing.yml file and distribute
them with the bundle.


The autoload.json file
----------------------

The autoload.json file must be placed in bundle's top folder and it is structured as
follows:

1. bundles (mandatory)
2. routing

The mandatory **bundles** section contains the bundles you want to autoload. Let's 
see a very basic example:

.. code-block:: text

    {
        "bundles" : {
            "RedKiteLabs\\Block\\BusinessDropCapBundle\\BusinessDropCapBundle" : ""
        }
    }

This autoloads the BusinessDropCapBundle for the whole application's environments.


Environments
~~~~~~~~~~~~

When you need to autoload a bundle only for certains environments, just add the 
**environments** option to the bundle:

.. code-block:: text

    {
        "bundles" : {
            "RedKiteLabs\\Block\\BusinessDropCapBundle\\BusinessDropCapBundle" : {
                "environments" : ["dev", "test"]
            }
        }
    }

The **environments** option enables the bundle only for the declared environments. In 
the example above, the BusinessDropCapBundle is enabled only for the dev and test enviroments.


The all keyword
~~~~~~~~~~~~~~~

To specifiy all the enviroments you can use the **all** keyword:

.. code-block:: text

    {
        "bundles" : {
            "RedKiteLabs\\Block\\BusinessDropCapBundle\\BusinessDropCapBundle" : {
                "environments" : ["all"]
            }
        }
    }

.. note::

    This is the default value for the the **environments** option


The **overrides** options
~~~~~~~~~~~~~~~~~~~~~~~~~

When a bundle overrides another bundle, it must be instantiated in the AppKernel after 
the overrided bundle.

You can implement this case in your autoload.json adding the **overrides** option:

.. code-block:: text

    {
        "bundles" : {
            "RedKiteLabs\\Block\\BusinessDropCapBundle\\BusinessDropCapBundle" : {
                "environments" : ["dev", "test"],
                "overrides" : ["BusinessCarouselBundle"]
            }
        }
    }

In this example the **bundles order** will be resolved instantiating the BusinessCarouselBundle 
**before** the BusinessDropCapBundle


Autoloading a bundle without the autoloader.json file
-----------------------------------------------------

You might wonder why we are talking about "bundles" and not just "bundle". This is 
quite simple to explain, in fact you could autoload a bundle that does not implement
the autoloader.json file.

For example, let's suppose the **BusinessCarouselBundle requires the PropelBundle to work** 
and this last bundle does not implement the autoloader.json file.

In this case you can easily autoload the PropelBundle just declaring that bundle 
in you autoload.json file:

.. code-block:: text

    {
        "bundles" : {
            "RedKiteLabs\\Block\\BusinessDropCapBundle\\BusinessDropCapBundle" : {
                    "environments" : ["dev", "test"]
            },
            "Propel\\PropelBundle\\PropelBundle" : ""
        }
    }

If you need to enable it for specific environments, you just  have to add the **environments** 
option as explained above.

Another situation could be when a bundle implements a new environment. For example 
the RedKiteCmsBundle implements the rkcms and the rkcms_dev environments so we need
to register many thirdy part bundles for those envs:

.. code-block:: text

    {
        "bundles" : {
            "RedKiteLabs\\RedKiteCmsBundle\\RedKiteCmsBundle" : {
                "environments" : ["rkcms", "rkcms_dev", "rkcms_test", "test"]
            },
            "Propel\\PropelBundle\\PropelBundle" : {
                "environments" : ["rkcms", "rkcms_dev", "rkcms_test", "test"]
            },
            "Symfony\\Bundle\\WebProfilerBundle\\WebProfilerBundle" : {
                "environments" : ["rkcms_dev", "rkcms_test"]
            },
            "Sensio\\Bundle\\DistributionBundle\\SensioDistributionBundle" : {
                "environments" : ["rkcms_dev", "rkcms_test"]
            },
            "Sensio\\Bundle\\GeneratorBundle\\SensioGeneratorBundle" : {
                "environments" : ["rkcms", "rkcms_dev", "rkcms_test"]
            }
        }
    }

The **"RedKiteLabs\\RedKiteCmsBundle\\RedKiteCmsBundle"** section enables the RedKiteCmsBundle
for the **"rkcms", "rkcms_dev", "rkcms_test", "test"**, then requires the PropelBundle for the
same environments and at last requires the WebProfilerBundle, the SensioDistributionBundle
and the SensioGeneratorBundle for the **"rkcms_dev", "rkcms_test"** environments.


Configuration files (config.yml - routing.yml)
----------------------------------------------

Usually each bundle requires to add some configurations inside the application's config.yml to 
make it work properly. Some of these settings could be generic, i.e. enabling the bundle to use 
assetic, while others could be specific for the application which is using that bundle.

The **BootstrapBundle** let the developer to define the generic settings directly with the bundle. 
This will produce some benefits for the final user:

1. The bundle that requires only generic setting is ready to be used without touching the application's config.yml file
2. When the bundle is used by many applications, the generic configuration is already made
3. Less frustation for the user
4. Less frustation for the bundle's developer who has to write less documentation
5. Light config.yml file

To add a configuration that usually goes into the application's **config.yml** file, 
just add a **config.yml** file under the **Resources/config** folder of your bundle 
and add the required setting to it.

The BootstrapBundle takes care to copy it into the **app/config/bundles/[environment]** 
folder and load your configuration in the AppKernel class.

A practical example
-------------------

For example, RedKiteCmsBundle uses assetic to manage its assets, so the user who want 
to use that bundle should add the following configuration to the config.yml file of 
his application:

.. code-block:: text

    app/config/config.yml

    assetic:
    bundles: [RedKiteLabsCmsBundle]
    filters:
        cssrewrite: ~
        yui_css:
            jar: %kernel.root_dir%/Resources/java/yuicompressor.jar
        yui_js:
            jar: %kernel.root_dir%/Resources/java/yuicompressor.jar

With the BootstrapBundle these setting have been added to RedKiteCmsBundle's config.yml 
file so the user is not required to manually add those settings to application's config.yml.

The same concepts are applied to the routes implemented by the bundle, so you can add 
a **routing.yml** file into the **Resources/config** of your bundle and the BootstrapBundle 
will do the rest for you.

Routing priority
~~~~~~~~~~~~~~~~

When you need to assign a specific priority to routing files, you can add a **routing/priority**
setting to your configuration file:

.. code-block:: text

    {
        "bundles" : {
            "RedKiteLabs\\Block\\BusinessDropCapBundle\\BusinessDropCapBundle" : ""
        },
        "routing" : {
            "priority" : "128"
        }
    }

Each bundle gets zero as routing priority when the option is not specified. To load 
the routing file after, specify a value higher than zero, to load the routing file before, 
specify a value lower than zero.


Found a typo? Is something not correct in this documentation? `Just fork and edit it!`_

.. _`Just fork and edit it!`: https://github.com/redkite-labs/redkitecms-docs