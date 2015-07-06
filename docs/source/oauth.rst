Creació d'instàncies de oauth
=============================

Passos per afegir una instància nova de max a |oauth_server|. Prèviament a crear
la instància nova, necessitem saber quin ldap farà servir aquesta instància.

Si la instància que volem crear tindrà el mateix nom que una branca d'ldap que hem
creat previament amb el mateix nom només caldrà executar::

    gum oauth add instance {instance_name}

si per altra banda, volem utilizar un ldap amb un nom diferent, ho haurem d'especificar::

    gum oauth add instance {instance_name} --ldap-branch={branch_name}

Un cop acabat el procés, la instància estarà creada però sense arrencar. Per arrencar-la::

    gum oauth {instance_name} start

Per últim, hem de recarregar la configuració d'nginx nova que s'ha generat per aquestoauth::

    gum oauth reload nginx

Per qualsevol error que pugui apareixer en la execució de les comandes, es pot continuar
la creació de la instància des del punt en que ho ha deixat el gum de manera manual, seguint les :doc:`oauth_manual`

.. toctree::
   :maxdepth: 2

   oauth_manual.rst
