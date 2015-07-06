Instal·lació del gum
======================

Passos per instal·lar el gum en local:

Primer ens assegurem que tenim totes els paquets necessaris al sistema::

    apt-get install python-dev build-essential libldap2-dev libsasl2-dev libxml2-dev libxslt1-dev

Per install·lar el gum necessitem un python >= 2.7, el de sistema segurament ja compleix aquest requeriment. Podem comprovar-ho amb::

    python --version

Instal·lem el gestor de paquets del python::

     wget https://bootstrap.pypa.io/ez_setup.py
     python ez_setup.py
     easy_install pip

I instal·lem el paquet **virtualenv** per crear una copia aïllada del python::

    easy_install virtualenv

Escollim un lloc on instalar aquesta copia aïllada, d'aqui en endavant assumirem ``~/gum``::

    virtualenv ~/gum --no-site-packages

A partir d'ara, el binari de python que utilitzarem és el que s'haura generat a ``~/gum/bin/python``. Amb aquest, instal·lem el gum::

    ~/gum/bin/pip install --extra-index-url=http://pypi.upc.edu gummanager.cli

Per poder accedit còmodament al gum, podem crear un enllaç simbòlic en algun lloc del nostre ``PATH``, per exemple::

    ln -s ~/gum/bin/gum /usr/sbin/gum

Per últim, aconsegueix una copia del fitxer de configuració ``.gum.conf``, i guarda-la a ``~/.gum.conf``


Actualització del gum
----------------------

Per actualitzar el gum i les seves dependències::

    ~/gum/bin/pip install --upgrade --extra-index-url=http://pypi.upc.edu gummanager.cli

