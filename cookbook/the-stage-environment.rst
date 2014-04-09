The stage environment
=====================

RedKite CMS stage environment, lives between the backend and production environments,
and it is the place where you can verify that the website you are developing works
exactly as you expected, before releasing it to production.

RedKite CMS keeps completely separated the backend from the production environment,
so all the changes you made inside the backend, including changes to contents, adding
and removing assets like images or any kind of file, will be available in production
only when you explicitly deploy the website for production.

You cannot roll back from a website deploy and this could be a problem if something
goes wrong or something does not work as you expected, when you have deployed in production.

Here is where the stage environment comes into play, because you can use this environment
to check that everything works fine.

When you deploy the website for the stage environment, RedKite CMS makes all the 
operations it would do for the production environment, for the stage one, so
you can verify how your website will work in production without any risk, before the
real deploy.


Deploy the stage environment
----------------------------
Deploy for the stage environment is easy as clicking on the **Deploy Stage** button and
then confirming the operation.

.. note::

    Running this command is always safe, but RedKite CMS asks for confirmation
    because this task takes some time.


Work on the stage environment
-----------------------------
The stage environment is accessible using the **stage.php** front controller. The best way
to interact with it, is to open a new tab on your browser, usual using the **t+T combination**,
and point the stage environment from there:

.. code::

    http://localhost/stage.php

If you need to have debug information, simply use the **stage_dev.php** front controller,
instead of the stage.php one


Working with the server
-----------------------
