Instruccions pas a pas
======================

A continuació hi ha totes les instruccions necessaries per crear una branca de
ldap manualment. Les instruccions aquí recollides són a nivell de quines estructures s'han
de crear a l'LDAP, i no fan referència a cap eina concreta.



Afegir OU a l'arrel
--------------------

Cada branca és representada per una ou a l'arrel, ``ou=nombranca, dc=upcnet, dc=es`` amb els seguents paràmetres:

.. parsed-literal::

    **objectClass**: top
    **objectClass**: organizationalUnit
    **ou**:          nombranca
    **description**: nombranca

Creació d'usuaris administradors
--------------------------------

El primer usuari que hem de crear sera l'usuari |ldap_branch_admin_cn|, que estarà ubicat a l'arrel de la nova ou que acabem de crear. El dn de l'usuari serà ``cn=ldap, ou=nombranca, dc=upcnet, dc=es``. Aquest usuari serà l'encarregat de crear usuaris a dins de la OU:

.. parsed-literal::

    **objectClass**:  top
    **objectClass**:  organizationalPerson
    **objectClass**:  person
    **objectClass**:  inetOrgPerson
    **cn**:           username
    **sn**:           LDAP Access User
    **userPassword**: <sha1 encrypted password>

A continuació crearem un altre usuari al mateix lloc amb el cn ``restricted`` i el sn ``Restricted User``. Aquest usuari també podrà crear usuaris a dins la branca, pero tindrà menys privilegis que el |ldap_branch_admin_cn|.

Creació de rols i assignació d'usuaris
---------------------------------------

A l'arrel de l'OU hem de crear el grup ``Managers``, amb dn ``cn=Managers, ou=nombranca, dc=upcnet, dc=es``, on hi assignarem l'usuari ``ldap`` que hem creat previament.

.. parsed-literal::

    **objectClass**:  top
    **objectClass**:  groupOfNames
    **cn**:           nom_del_grup
    **member**:       cn=ldap,ou=nombranca,dc=upcnet,dc=es

Creació de contenidors per usuaris i grups
------------------------------------------

Els usuaris finals i grups que es necessitin des del serveis que utilizaran la branca,
quedaran guardats en dues OUs aniuades a dins la principal. Els paràmetres de la ou són els mateixos utilitzats en crear la ou principal, utilitzant com a ``nom_ou`` els noms ``groups`` i ``users``.

.. parsed-literal::

    **objectClass**: top
    **objectClass**: organizationalUnit
    **ou**:          nom_ou
    **description**: nom_ou

Creació d'usuaris per defecte
-----------------------------

Per últim, cada branca tindrà per defecte uns usuaris creats amb l'objectiu de tenir usuaris de prova generics. Els usuaris s'han de crear amb el dn ``cn=nom_usuari, ou=users, ou=nombranca, dc=upcnet, dc=es``. La llista d'usuaris inicials que s'ha de crear és la següent:

.. parsed-literal::

    upcnet.manteniment
    ulearn.user1
    ulearn.user2
    ulearn.user3

Les propietats de cada usuari són les mateixes descrites al crear els usuaris administradors.



