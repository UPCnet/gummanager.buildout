Instruccions pas a pas
======================

A continuació hi ha totes les instruccions necessaries per crear una instània de
oauth manualment, agrupades segons la comanda de gum, i els passos interns de cada comanda,
si n'hi han.


Passos previs
-------------

Entrar al servidor on hi han les instàncies de oauth:

.. parsed-literal::

    ssh |oauth_ssh_user_c| @ |oauth_server_c|

LListar els ports utilitzats per les instàncies existents, i escollir-ne un de lliure.
Els ports són basats en ``10000``.

.. parsed-literal::

    cd |oauth_instances_root_c|
    find -wholename '\*/config/osiris.ini' -exec sed -n 's/port = \([0-9]*\)/\1/p' '{}' \; | sort -n

gum oauth add instance {instance_name}
--------------------------------------

A partir d'aquí, tindrem ja determinats el nom de la nova instància ``{instance_name}``, el
valor del port lliure ``{port_index}``i el nom de la instància de oauth ``{oauth_name}``
( per defecte el mateix que ``{instance_name}``)

Cloning buildout
................

Fer una copia local del buildout des del repositori.

.. parsed-literal::

    cd |oauth_instances_root_c|
    git clone git@github.com:UPCnet/maxserver.git {instance_name}


Bootstraping buildout
.....................

Preparar l'entorn del buildout avans de l'execució.

.. parsed-literal::

    cd |oauth_instances_root_c|/{instance_name}
    |oauth_python_interpreter_c| bootstrap.py -c osiris-only.cfg

Configuring customizeme.cfg
...........................

Editar l'axiu |oauth_instances_root| ``/{instance_name}/customizeme.cfg``, omplint les dades segons la instància
que s'està afegint.

.. parsed-literal::

    [hosts]
    main = |oauth_server_dns_c|,
    rabbitmq = |oauth_rabbitmq_server_c|,
    mongodb_cluster = |oauth_mongodb_cluster_c|

    [max-config]
    name = {instance_name},

    [ports]
    port_index =  {port_index}

Executing buildout
..................

Executem el buildout utilitzant el fitxer que instancia només les parts necessàries per un oauth

.. parsed-literal::

    ./bin/buildout -c osiris-only.cfg

Configuring ldap.ini
.........................

:cfgfile:`LDAP_INI,ini`


Creating nginx entry for oauth
...............................

Crear l'axiu |oauth_nginx_root| ``/config/osiris-instances/{instance_name}.conf``, substituint les dades en la
següent plantilla segons les dades de la instància que estem afegint. El valor de ``{oauth_port}`` serà el resultant
de sumar el port base |ports_OSIRIS_BASE_PORT| al ``{port_index}`` escollit.

:cfgfile:`OAUTH_NGINX_ENTRY,nginx`


Creating nginx entry for circus
...............................

Crear l'axiu |oauth_nginx_root| ``/config/circus-instances/{instance_name}.conf``, substituint les dades en la
següent plantilla segons les dades de la instància que estem afegint.

El valor de ``{circus_nginx_port}`` serà el resultant
de sumar el port base |ports_CIRCUS_NGINX_BASE_PORT| al ``{port_index}`` escollit.

El valor de ``{circus_httpd_endpoint}`` serà el resultant
de sumar el port base |ports_CIRCUS_HTTPD_BASE_PORT| al ``{port_index}`` escollit.

:cfgfile:`CIRCUS_NGINX_ENTRY,nginx`


Creating init.d script
......................

Crear l'axiu ``/etc/init.d/osiris_{instance_name}``, substituint les dades en la
següent plantilla segons les dades de la instància que estem afegint.

:cfgfile:`INIT_D_SCRIPT,bash`

Commiting to local branch
.........................

Crear una branca de git local de nom igual al servidor, que servira per guardar la configuració
especifica de cada instància, i poder-ne tenir un històric.

.. parsed-literal::

    git checkout -b |oauth_local_git_branch_c|
    git commit -m "Setup custom configuration"


Changing permissions
....................

Com que totes les operacions s'han fet com a root, cal deixar el sistema de fitxers amb els permisos posats com a l'usuari que executarà el servei.

.. parsed-literal::

    chown -R  |oauth_process_uid_c|:|oauth_process_uid_c|.



gum oauth {instance_name} start
-------------------------------

Arrencar la instància d'oauth

.. parsed-literal::

    /etc/init.d/oauth_{instance_name} start

gum oauth nginx reload
-----------------------------

Recarregar configuració de nginx per tal que agafi el nou fitxer de configuració. Primer testejarem que no hi hagi cap error, i només si esta tot correcte, reiniciarem l'nginx

.. parsed-literal::

    /etc/init.d/nginx configtest
    /etc/init.d/nginx reload


