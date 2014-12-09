Installation
============

Mac or Linux
------------

If you have python 2.7 installed on your system.

Make sure you have virtualenv installed:

..  code-block:: bash

    $ pip install -U virtualenv

Go to where you want to create the folder for the dojo.

..  code-block:: bash

    $ cd <whereever you want>

In that folder clone the buildout for the dojo:

..  code-block:: bash

    $ git clone https://github.com/starzel/plone-dojo.git buildout
    $ cd buildout
    $ virtualenv-2.7 py27
    $ ./py27/bin/pip install -U setuptools==6.1
    $ ./py27/bin/python bootstrap.py
    $ ./bin/buildout

This will take **a lot of time** and produce a lot of output because it downloads and configures Plone.


Windows
-------

Use `VirtualBox <https://www.virtualbox.org>`_ and `Vagrant <http://www.vagrantup.com>`_ to run Linux in a virtual machine.

See :doc:`vagrant` for instructions.


No python installed
-------------------

Same as Windows: Use `VirtualBox <https://www.virtualbox.org>`_ and `Vagrant <http://www.vagrantup.com>`_. See :doc:`vagrant`.
