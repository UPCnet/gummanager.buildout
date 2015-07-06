Esborrat d'instàncies de oauth
==============================

Passos per esborrar una instància nova de oauth a |oauth_server|.

Entrar al servidor on hi han les instàncies de oauth:

.. parsed-literal::

    ssh |oauth_ssh_user_c| @ |oauth_server_c|


Parar instància::

    /etc/init.d/oauth_{instance_name} stop


Esborrar carpeta de la instància:

.. parsed-literal::

    cd |oauth_instances_root_c|
    rm -rf {instance_name}


Esborrar entrades de nginx:

.. parsed-literal::

    rm |oauth_nginx_root_c|/config/osiris-instances/{instance_name}.conf
    rm |oauth_nginx_root_c|/config/circus-instances/{instance_name}.conf


Esborrar script d'arrancada::

    update-rc.d -f oauth_{instance_name} remove
    rm /etc/init.d/oauth_{instance_name}

Esborrar base de dades::

    /var/mongodb/bin/mongo
    use oauth_{instance_name}
    db.dropDatabase()
