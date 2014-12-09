Coding
======

Edit ``browser/dojo.py``.

Use the method ``dojo`` to return something else.

.. code-block:: python

    def dojo(self):
        """Add code here.
        """
        context = self.context
        title = context.title
        portal_type = context.portal_type
        url = context.absolute_url()
        return "This is the {0} {1} at {2}".format(portal_type, title, url)

The template ``dojo.pt`` still needs the initial tal-statement:

.. code-block:: xml

    <h1 tal:content="python: view.dojo()">
        Hello World!
    </h1>

Changed python-files are used by restarting Plone or http://localhost:8080/@@reload


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


Exercise 1
++++++++++

Create 5 talks each time the new method ``create_talks`` is called.

* Use the docs at http://docs.plone.org/external/plone.api/docs/content.html#create-content to find out how to do that.
* Use ``logger.info("something")`` to print log-messages to the console.

..  admonition:: Solution
    :class: toggle

    .. code-block:: python

        # -*- coding: UTF-8 -*-
        from Products.Five.browser import BrowserView
        from plone import api

        import logging
        logger = logging.getLogger(__name__)


        class DojoView(BrowserView):
            """A browser view.
            """

            def create_talks(self, amount=5):
                """Create talks
                """
                portal = api.portal.get()

                for i in range(amount):
                    obj = api.content.create(
                        type='talk',
                        title='A talk',
                        container=portal)
                    logger.info("Created talk {0}".format(obj.absolute_url()))


For many **bonus-points**: Allow the user to pass the number of talks to be created as a query-string.

* Query-strings are automatically passed as parameters to the ``__call__`` method.
* That method again is responsible for rendering the template. Now you have to do this now by hand.
* Register the template as a instance of ``Products.Five.browser.pagetemplatefile.ViewPageTemplateFile`` (it takes repative paths) and call it as ``return self.template()``.

..  admonition:: Solution
    :class: toggle

    .. code-block:: python

        # -*- coding: UTF-8 -*-
        from Products.Five.browser import BrowserView
        from Products.Five.browser.pagetemplatefile import ViewPageTemplateFile
        from plone import api

        import logging
        logger = logging.getLogger(__name__)


        class DojoView(BrowserView):
            """A browser view.
            """

            template = ViewPageTemplateFile("dojo.pt")

            def __call__(self, amount=5):
                self.amount = int(amount)
                return self.template()

            def create_talks(self):
                """Create talks
                """
                portal = api.portal.get()
                for i in range(self.amount):
                    obj = api.content.create(
                        type='talk',
                        title='A talk',
                        container=portal)
                    logger.info("Created talk {0}".format(obj.absolute_url()))


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



Exercise 2
++++++++++

Find all private talks in the page and publish them. Display a html-list of the published items.

* Use the tool ``portal_catalog`` to query for types.
* Use ``plone.api.content.transition`` to publish.
* There are some pittfalls ;-)

..  admonition:: Solution
    :class: toggle

    .. code-block:: python

        # ...

        class DojoView(BrowserView):

            # ...

            def publish_all_talks(self):
                """Publish all private talks
                """
                results = []
                portal_catalog = api.portal.get_tool('portal_catalog')
                brains = portal_catalog(
                    portal_type="talk",
                    review_state="private",
                )
                for brain in brains:
                    obj = brain.getObject()
                    api.content.transition(obj, to_state='published')
                    results.append(obj.absolute_url())
                    logger.info("Published talk {0}".format(obj.absolute_url()))

                return results

    Add this to the template ``dojo.pt``:

    .. code-block:: html

        <h2>Published talks:</h2>
        <ul tal:define="talks python:view.publish_all_talks()">
            <li tal:repeat="talk talks"
                tal:content="talk">
            </li>
            <li tal:condition="not: talks">
                No talks published
            </li>
        </ul>


Look at ``views.py`` to see a more advanced example.



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
