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

