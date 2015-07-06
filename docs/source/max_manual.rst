Instruccions pas a pas
======================

A continuació hi ha totes les instruccions necessaries per crear una instània de
max manualment, agrupades segons la comanda de gum, i els passos interns de cada comanda,
si n'hi han.


Passos previs
-------------

Entrar al servidor on hi han les instàncies de max:

.. parsed-literal::

    ssh |max_ssh_user_c| @ |max_server_c|

LListar els ports utilitzats per les instàncies existents, i escollir-ne un de lliure.
Els ports són basats en ``10000``.

.. parsed-literal::

    cd |max_instances_root_c|
    find -wholename '\*/config/max.ini' -exec sed -n 's/port = \([0-9]*\)/\1/p' '{}' \; | sort -n

gum max add instance {instance_name}
------------------------------------

A partir d'aquí, tindrem ja determinats el nom de la nova instància ``{instance_name}``, el
valor del port lliure ``{port_index}``i el nom de la instància de oauth ``{oauth_name}``
( per defecte el mateix que ``{instance_name}``)

Cloning buildout
................

Fer una copia local del buildout des del repositori.

.. parsed-literal::

    cd |max_instances_root_c|
    git clone git@github.com:UPCnet/maxserver.git {instance_name}


Bootstraping buildout
.....................

Preparar l'entorn del buildout avans de l'execució.

.. parsed-literal::

    cd |max_instances_root_c|/{instance_name}
    |max_python_interpreter_c| bootstrap.py -c max-only.cfg

Configuring customizeme.cfg
...........................

Editar l'axiu |max_instances_root| ``/{instance_name}/customizeme.cfg``, omplint les dades segons la instància
que s'està afegint.

.. parsed-literal::

    [hosts]
    main = |max_server_dns_c|,
    rabbitmq = |max_rabbitmq_server_c|,
    mongodb_cluster = |max_mongodb_cluster_c|

    [max-config]
    name = {instance_name},

    [ports]
    port_index =  {port_index}

    [urls]
    oauth = https://|max_server_c|/{oauth_name}
    rabbit = amqp://|utalk_rabbitmq_username_c|:{rabbitmq_password} @ |utalk_server_c|:|utalk_port_c|/%2F


Executing buildout
..................

Executem el buildout utilitzant el fitxer que instancia només les parts necessàries per un max

.. parsed-literal::

    ./bin/buildout -c max-only.cfg

Adding indexes to mongodb
.........................

Inicialitzar els index a la base de dades que anirà associada a aquest max, executant el següent
script que ho inicialitza:

.. parsed-literal::

    ./bin/max.mongoindexes -c config/max.ini -i config/mongodb.indexes


Configuring default permissions settings
........................................

Per al correcte funcionament, es necessita com a mínim un superusuari al arrencar. Per defecte aquest usuari
s'anomena ``restricted``. Podem editar el fitxer ``config/.authorized_users`` per determinar quins seran els usuaris
que seran superusuaris. Un cop editat el fitxer, cal aplicar-ho a la base de dades:

.. parsed-literal::

    ./bin/initialize_max_db config/max.ini

Creating nginx entry for max
............................

Crear l'axiu |max_nginx_root| ``/config/max-instances/{instance_name}.conf``, substituint les dades en la
següent plantilla segons les dades de la instància que estem afegint. El valor de ``{max_port}`` serà el resultant
de sumar el port base |ports_MAX_BASE_PORT| al ``{port_index}`` escollit.

:cfgfile:`MAX_NGINX_ENTRY,nginx`


Creating nginx entry for circus
...............................

Crear l'axiu |max_nginx_root| ``/config/circus-instances/{instance_name}.conf``, substituint les dades en la
següent plantilla segons les dades de la instància que estem afegint.

El valor de ``{circus_nginx_port}`` serà el resultant
de sumar el port base |ports_CIRCUS_NGINX_BASE_PORT| al ``{port_index}`` escollit.

El valor de ``{circus_httpd_endpoint}`` serà el resultant
de sumar el port base |ports_CIRCUS_HTTPD_BASE_PORT| al ``{port_index}`` escollit.

:cfgfile:`CIRCUS_NGINX_ENTRY,nginx`


Creating init.d script
......................

Crear l'axiu ``/etc/init.d/max_{instance_name}``, substituint les dades en la
següent plantilla segons les dades de la instància que estem afegint.

:cfgfile:`INIT_D_SCRIPT,bash`

Commiting to local branch
.........................

Crear una branca de git local de nom igual al servidor, que servira per guardar la configuració
especifica de cada instància, i poder-ne tenir un històric.

.. parsed-literal::

    git checkout -b |max_local_git_branch_c|
    git commit -m "Setup custom configuration"


Changing permissions
....................

Com que totes les operacions s'han fet com a root, cal deixar el sistema de fitxers amb els permisos
posats com a l'usuari que executarà el servei.

.. parsed-literal::

    chown -R  |max_process_uid_c|:|max_process_uid_c|.


Adding instance to bigmax
.........................

Per cada instància nova, se n'ha d'informar al bigmax afegint la entrada corresponent al fitxer de instàncies
ubicat a |max_bigmax_instances_list|. Cada entrada ha de seguir la seguent plantilla:

:cfgfile:`BIGMAX_INSTANCE_ENTRY,ini`


gum max {instance_name} start
-----------------------------

Arrencar la instància de max

.. parsed-literal::

    /etc/init.d/max_{instance_name} start

gum max nginx reload
-----------------------------

Recarregar configuració de nginx per tal que agafi el nou fitxer de configuració. Primer testejarem
que no hi hagi cap error, i només si esta tot correcte, reiniciarem l'nginx

.. parsed-literal::

    /etc/init.d/nginx configtest
    /etc/init.d/nginx reload


