Creació de branques ldap
========================

Passos per afegir una branca d'ldap a |ldap_server|

Crear la branca executant la comanda::

    gum ldap add branch nombranca


Si consultem les branques existents, veurem que hi ha la nova branca, amb 4 usuaris creats::

    gum ldap nombranca list users


Per qualsevol error que pugui apareixer en la execució de les comandes, es pot continuar
des del punt en que ho ha deixat el gum de manera manual, seguint les :doc:`ldap_manual`


.. toctree::
   :maxdepth: 2

   ldap_manual.rst
