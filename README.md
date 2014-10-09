gummanager.buildout
===================

Development enviornment setup
-----------------------------

To user the development buildout, first instantiate it

::
    git clone git@github.com:UPCnet/gummanager.buildout.git gum
    cd gum
    python bootstrap.py
    ./bin/buildout

To use gum devel cli command, execute this inside the buildout folder::

    ./bin/gum

To build the documentation, execute::

    ./bin/sphinxbuilder

This will build html and pdf version on ``src/docs/build/html`` and ``src/docs/build/latex/GUMManager.pdf``


CLI Setup
---------

To install gum command line interface, please follow this steps:

Install system package dependencies::

    apt-get install python-dev build-essentials libldap2-dev libsasl2-dev libxml2-dev libxslt1-dev

Install setuptools and pip on your chooser python if not present::

     wget https://bootstrap.pypa.io/ez_setup.py
     python ez_setup.py
     easy_install pip

Install virtualenv on your choosen python (>=2.7)::

    easy_install virtualenv

Setup a new virtualenv on a ``PATH`` (wherever you want)::

    virtualenv PATH

From now on, the python interpreter, and pip to acces that new env
will be the ones in PATH/bin/*.
Install ``gummanager.cli`` package with the following command::

    PATH/bin/pip install --extra-index-url=http://pypi.upc.edu gummanager.cli

Add symlinks to be able to execute command directly::

    ln -s PATH/bin/gum /usr/sbin/gum

Finally ask the appropiate person to provide you with a .gum.conf file with
the settings you'll need, and place it on your system's home folder.


Dependencies
------------

To build documentation you'll need this packages on your system:

    * texlive-latex-extra

