Creació d'instàncies de utalk
=============================

Realment no es creen noves instàncies de utalk, sinó el que es fa és informar al utalk de la presència de noves instàncies de max, que utilitzaràn el servei utalk. Per referenciar un max nou al utalk (d'ara en endavant **domini**)::

    gum utalk add instance {instance_name} --hastag={hashag} --language={language}

Si volem activar el servei de twitter per aquest domini, haurem de fer-ho especificant el hashtag escollit per el client (sense el #), sino passem el paràmetre ``--hashtag`` quedara deshabilitat.

El parametre language, fa referència a l'idioma en que apareixaran alguns missatges gernèrics de les aplicacions mòbils, i pot ser ``ca``, ``en`` o ``es``.

Per qualsevol error que pugui apareixer en la execució de les comandes, es pot continuar
des del punt en que ho ha deixat el gum de manera manual, seguint les :doc:`utalk_manual`


.. toctree::
   :maxdepth: 2

   utalk_manual.rst
