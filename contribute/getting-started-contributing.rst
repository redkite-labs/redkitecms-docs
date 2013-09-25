Developer Guide
===============

This guide is intended for those who wish to:

- Contribute code to RedKite CMS core
- Contribute code to RedKite CMS ecosystem
- Contribute code to RedKite CMS documentation    
- Contribute code to RedKite CMS existing Blocks and Themes
- Create their own Blocks and Themes and share them on RedKite CMS website

In order to work with RedKite CMS as a developer, it's recommended that:

- You know **php5** and **Object Oriented Programming**, since RedKite CMS is written in php.
- You know **Symfony2**, since RedKite CMS is a **Symfony2 Bundle**.
- While it is not mandatory, it is better if you are comfortable with **Propel**, since RedKite CMS uses it as ORM.    
- RedKite CMS stylesheets are generated from **Sass** and compiled with **Compass**, so you should have that tool installed on your system.
- You're comfortable with **command-line programs**.
- You understand unit and functional tests and why they're important.
- You know **PHPUnit**, since RedKite CMS tests are written for this testing suite.
- Javascript dependencies are managed with **Twitter's Bower**, so you should install it.

RedKite Labs licensing philosophy
---------------------------------

Although **Symfony2** and a huge part of its ecosystem is license under a **MIT License**,
which permits to use and redistribute the software both in Open Source and proprietary 
contexts, **RedKite CMS** is licensed under the **GPL2 License**, which means that it can be 
used both in an Open Source or in a proprietary context, but changes and derived redistributions
must be released as Open Source software under the same license used by RedKite CMS.

RedKite CMS final output consists in a set of Twig templates and Symfony2 routing
configuration files which normally would be licensed under the GPL2 license, because they
are generated with a GPL software.

Despite of that, we create and exception for those files, and we choose the MIT license for them.

This is possible because a Symfony2 application powered by RedKite CMS, does not require
the CMS and the App-Blocks to run in production, so in this case, the viral effect of GPL 
license is cancelled.

Open sourced App-Blocks should be released under the MIT license or another permissive license
like the LGPL, while they can be released under the GPL2 because, as for RedKite CMS, they 
are not required by the final Symfony application.

Open sourced App-Themes must be released under the MIT License or equivalent because they are
required by the final Symfony application.

Summarizing:

- RedKite CMS Bundle -> GPL2 License
- RedKite CMS final output -> MIT  License
- App-Blocks -> MIT/LGPL preferred, GPL2 possible
- Theme-Blocks -> MIT preferred, but an equivalent license can be used
    
If that sounds like you, then continue reading to get started.

Section 1: Get the Source Code
------------------------------
Before you can get started, you'll need to get a copy of the RedKite CMS source code. 
This section explains how to do that and a little about the source code structure.

`Go to source code section`_

Section 2: Run the tests
------------------------
There are a lot of unit tests included with RedKite CMS to make sure that we're keeping 
on top of code quality. This section explains how to run the unit tests.

`Go to run the tests section`_

Section 3: Contributing
-----------------------
Once you've made changes that you want to share with the community, the next step is 
to submit those changes back via a pull request.

`Go to contributing section`_


.. class:: fork-and-edit

Found a typo ? Something is wrong in this documentation ? `Just fork and edit it !`_

.. _`Just fork and edit it !`: https://github.com/redkite-labs/redkitecms-docs
.. _`Go to source code section`: how-to-get-alphalemon-cms-source-code-and-bundle-structure
.. _`Go to run the tests section`: https://github.com/redkite-labs/redkitecms-docs
.. _`Go to contributing section`: https://github.com/redkite-labs/redkitecms-docs