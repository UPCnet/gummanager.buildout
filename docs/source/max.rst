Creació d'instàncies de max
============================

Passos per afegir una instància nova de max a |max_server|. Prèviament a crear
la instància nova, necessitem saber quin oauth farà servir aquesta instància.

Si la instància que volem crear tindrà el mateix nom que una instància d'oauth que hem
creat previament amb el mateix nom només caldrà executar::

    gum max add instance nominstancia

si per altra banda, volem utilizar un oauth amb un nom diferent, ho haurem d'especificar::

    gum max add instance nominstancia --oauth-instance=nomoauth

Un cop acabat el procés, la instància estarà creada però sense arrencar. Per arrencar-la::

    gum max nominstancia start

Per últim, hem de recarregar la configuració d'nginx nova que s'ha generat per aquest max::

    gum max reload nginx

Per qualsevol error que pugui apareixer en la execució de les comandes, es pot continuar
la creació de la instància des del punt en que ho ha deixat el gum de manera manual, seguint les :doc:`max_manual`


Si per aquesta instància de mas s'ha d'activar el servei de utalk, activar-lo seguint les instruccions de :doc:`utalk`

.. toctree::
   :maxdepth: 2

   max_manual.rst
