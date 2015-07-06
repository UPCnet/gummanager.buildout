Esborrat d'instàncies de max
==============================

Passos per esborrar una instància nova de max a |max_server|.

Entrar al servidor on hi han les instàncies de max:

.. parsed-literal::

    ssh |max_ssh_user_c| @ |max_server_c|


Parar instància::

    /etc/init.d/max_{instance_name} stop


Esborrar carpeta de la instància:

.. parsed-literal::

    cd |max_instances_root_c|
    rm -rf {instance_name}


Esborrar entrades de nginx:

.. parsed-literal::

    rm |max_nginx_root_c|/config/max-instances/{instance_name}.conf
    rm |max_nginx_root_c|/config/circus-instances/{instance_name}.conf


Esborrar script d'arrancada::

    update-rc.d -f max_{instance_name} remove
    rm /etc/init.d/max_{instance_name}

Esborrar base de dades::

    /var/mongodb/bin/mongo
    use max_{instance_name}
    db.dropDatabase()

Esborrar referències al max esborrat
------------------------------------

S'ha d'esborrar l'entrada de bigmax que fa referència al max que hem esborrat. Hem d'editar el fitxer que es troba a |max_bigmax_instances_list| al mateix servidor que el max i esborrar la secció amb nom ``[{instance_name}]``

També s'ha d'esborrar una entrada similar que existex al servidor del servei utalk. Entrarem al servidor

.. parsed-literal::

    ssh |utalk_ssh_user_c| @ |utalk_server_c|

i editarem el fitxer |utalk_maxbunny_instances_list|, on esborrarem la secció amb nom ``[max_{instance_name}]``
