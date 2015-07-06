Instruccions pas a pas
======================

A continuació hi ha totes les instruccions necessaries per actualitzar la informació del utalk després de la creació de instàncies de max noves.


Passos previs
-------------

Conèixer el servidor de max al qual s'ha d'associar l'utalk, així com el seu corresponent servidor oauth (per casos fora de l'estàndard de max externs.)

Entrar al servidor on hi ha l'instància de utalk, a la carpeta d'instal·lació:

.. parsed-literal::

    ssh |utalk_ssh_user_c| @ |utalk_server_c|
    cd /var/utalk.upcnet.es


Creació de l'entrada de utalk
------------------------------

Des d'aquesta carpeta d'instal·lació executarem un script que ens ajudarà a crear la entrada pel utalk, que s'afegirà al fitxer |utalk_maxbunny_instances_list| :

.. parsed-literal::

    ./bin/max.newinstance

L'script ens demanarà un seguit de preguntes semblants al llistat següent::

  > The name of the max instance (without max_ prefix): {name}
  > The base url of the max server [http://localhost:8081/{name}]:
  > The base url of the oauth server [https://oauth.upcnet.es/{name}]:
  > The hashtag to track on twitter (without #):
  > The restricted user: restricted
  > Password:

Per instal·lacions normals on tenim branca oauth i max amb el mateix nom ``{name}``, només cal que contestem la primera pregunta amb el nom de la instancia, i deixem les dues seguents buides, perque autogeneri les urls en funcio del ``{name}`` que haguem posat a la primera. Per casos especials on les urls del max i l'oauth no segueixin l'estandard, escriure les urls completes pels dos serveis.

El hashtag només caldrà informar-lo si aquest utalk tindrà el servei de twitter activat. En cas afirmatiu, posar el hastag sense el símbol del coxinet ``#``

L'usuari restringit serà l'usuari amb el qual el utalk executarà les peticions al max corresponent. Ha de ser un dels usuaris als quals s'hagi donat permisos de ``Manager`` al max, ``restricted`` com a valor estàndard.

Per últim, escriure el password de l'usuari restricted, per tal de poder obtenir el seu token. Un cop finalitzat, comprovarem que al final de l'arxiu de configuració |utalk_maxbunny_instances_list| s'ha afegit una entrada nova amb l'estructura següent:

:cfgfile:`MAXBUNNY_INSTANCE_ENTRY,ini`


