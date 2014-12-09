Views
=====


Browser Views
-------------

Open ``http://localhost:8080/Plone/dojo``.

``dojo`` is a browser view provided by the package ``plone.dojo``.

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


Templates
----------

Exercise 1
++++++++++

Change the template to display the title of the current object:

..  admonition:: Solution
    :class: toggle

    .. code-block:: html

        <h1 tal:content="python: context.title"></h1>


Exercise 2
++++++++++

For a talk:

* Render a mail-link to the speaker.
* Display the speaker instead of the raw email-adress.
* If there is no speaker-name display the adress.
* Use ``tal:attributes="<attr> <value>"`` to modify attributes of html-tags.

..  admonition:: Solution
    :class: toggle

    .. code-block:: html

        <a href=""
           tal:attributes="href python: 'mailto:{0}'.format(context.email)"
           tal:content="python: context.speaker if context.speaker else context.email">
           Mail the Speaker
        </a>


Exercise 3
++++++++++

Wrap the code in ``dojo.pt`` in the following code:

.. code-block:: xml

    <html metal:use-macro="context/main_template/macros/master">
    <metal:main fill-slot="content">

        <!-- Leave your code here! -->

    </metal:main>
    </html>

Replace the ``content` in ``fill-slot="content"`` with ``main`` and ``content-core`` and see what happens.

