Exercises and examples
======================

Addons
------

The addon ``plone.dojo`` is preinstalled by your buildout. You can find it in the ``src``-directory of the buildout.

We have to install it. Go to http://localhost:8080/Plone/prefs_install_products_form, select ``plone.dojo 0.1 `` and install it.

The code is in ``<buidout-directory>/src/plone.dojo/src/plone/dojo/``. All paths from now on are relative to this. The directory-structure is needed to make a package testable by itself.


Browser Views
-------------

The browser view ``http://localhost:8080/Plone/dojo`` is already there.

It is registered in ``browser/configure.zcml``

.. code-block:: xml

    <browser:page
        name="dojo"
        for="*"
        class=".dojo.DojoView"
        template="dojo.pt"
        permission="zope2.View"
        />

This refers to a python-class ``DojoView`` that is found in ``browser/dojo.py``

.. code-block:: python

    # -*- coding: UTF-8 -*-
    from Products.Five.browser import BrowserView
    from plone import api

    import logging
    logger = logging.getLogger(__name__)


    class DojoView(BrowserView):
        """A browser view.
        """

        def dojo(self):
            """Add code here.
            """

            return "Hello Plone!"

And to a template ``dojo.pt`` that is ``browser/dojo.pt``.

.. code-block:: html

    <h1 tal:content="python: view.dojo()">
        Hello World!
    </h1>


plone.api
---------

Covers 20% of the tasks any Plone developer does 80% of the time.

Check if plone.api first if you are not shure how to do omething.

The api is divided in five sections:

* `Content`: `Create content <http://docs.plone.org/external/plone.api/docs/content.html#create-content>`_
* `Portal`: `Send E-Mail <http://docs.plone.org/external/plone.api/docs/portal.html#send-e-mail>`_
* `Groups`: `Grant roles to group <http://docs.plone.org/external/plone.api/docs/group.html#grant-roles-to-group>`_
* `Users`: `Get user roles <http://docs.plone.org/external/plone.api/docs/user.html#get-user-roles>`_
* `Environment`: `Switch roles inside a block <http://docs.plone.org/external/plone.api/docs/env.html#switch-roles-inside-a-block>`_


portal-tools
------------

Some parts of Plone are very complex modules in themselves and have an api.

Here are a few examples:

portal_catalog
    ``unrestrictedSearchResults()`` returns search-results without checking if the current user has the permission to access the objects.

    ``uniqueValuesFor()`` returns all entries in a index

portal_setup
    ``runAllExportSteps()`` generates a tarball containing artifacts from all export steps.

portal_quickinstaller
    ``isProductInstalled()`` checks wether a product is installed.

Look in the ``interfaces.py`` in the respective package and read the docstrings.


Debugging
---------

tracebacks and the log
    The log (and the console when running in foreground) collect all log-messages Plone prints. When a exception occurs Plone thows a traceback. Most of the time the traceback is everything you need to find out what is going wrong. Also adding your own information to the log is very simple.

pdb
    The python debugger pdb is the single most important tool for us when programming. Just add ``import pdb; pdb.set_trace()`` in your code and debug away!

Products.PDBDebugMode
    A addon that has two killer-features.

    **Post-mortem debugging**: throws you in a pdb whenever a exception occurs. This way you can find out what is going wrong.

    **pdb-view**: simply adding ``/pdb`` to a url drops you in a pdb-session with the current context as ``self.context``. From there you can do just about anything.

Debug-mode
    When starting Plone using ``./bin/instance debug -O Plone`` you'll end up in a interactive debugger.

plone.reload
    An addon that allows to reload code that you changed without restarting the site. It is also used by plone.app.debugtoolbar.


Read more: http://plone-training.readthedocs.org/en/latest/api.html
