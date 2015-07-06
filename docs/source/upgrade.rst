Actualitzacions
===============

Per actualitzar el codi usat en els diferents sistemes, primer de tot hem de fer un parell
de passos comuns per tots els sistemes:

- Generar els eggs necessaris de l'actualització, i pujar-los a http://pypi.upc.edu
- Referenciar tots els canvis de versions que s'han d'aplicar al fitxer ``versions.cfg`` del repositori del maxserver a https://github.com/UPCnet/maxserver, i pujar-ho.


A partir d'aquí, s'ha de actuar en cada servidor per actualitzar el codi en cada instància. El procediment és pràcticament idèntic, de totes maneres el posem separat per cada servei per evitar confusions:

    * `Actualitzar una instància de max <#actualitzar-una-instancia-de-max>`__
    * `Actualitzar una instància d'oauth <#actualitzar-una-instancia-d-oauth>`__
    * `Actualitzar el utalk <#actualitzar-el-utalk>`__
    * `Actualitzar el bigmax <#actualitzar-el-bigmax>`__

Actualitzar una instància de max
--------------------------------

Les instàncies de max disposen de la comanda de gum::

    gum max {instance_name} upgrade


que pels casos mes comuns d'actualtizacions, executa tots els passos d'aquesta secció
amb les corresponents comprovacions, per cualsevol altre cas, repetir el procediment manual seguent per cada instància:

.. parsed-literal::

    ssh |max_ssh_user_c| @ |max_server_c|
    cd |max_instances_root_c|/{instance_name}
    git status


En aquest punt, hauriem de estar en la branca local |max_local_git_branch|, i ens hem d'assegurar que no hi ha canvis locals sense comitejar. En d'haver-n'hi, comitejar-los
primer avans de continuar amb el següent pas, recuperar els canvis del repositori

.. parsed-literal::

    git checkout master
    git pull
    git checkout |max_local_git_branch_c|
    git merge master

En actualitzacions simples, és poc probable que tinguem cap conflicte. En cas de tenir-ne
(per exemple si s'ha actualtizat els buildouts, o templates) resoldre els conflictes. En ambdós casos, comitejem els canvis del merge::

    git commit


Si hem de actualtizar configuració individual de la instància, aquest es el moment de fer-ho, i en acavat, comitejem els canvis de nou::

    git commit . -m "Local changes"


Tot seguit executem el buildout, deixem els permisos del sistema de fitxers en ordre,
i reiniciem el procés corresponent:

.. parsed-literal::

    ./bin/buildout -N -c max-only.cfg
    chown -R  |max_process_uid_c|:|max_process_uid_c|.
    /etc/init.d/max_{instance_name} restart



Actualitzar una instància d'oauth
---------------------------------

Per cada instància de max ``{instance_name}``, repetir el procediment

.. parsed-literal::

    ssh |oauth_ssh_user_c| @ |oauth_server_c|
    cd |oauth_instances_root_c|/{instance_name}
    git status


En aquest punt, hauriem de estar en la branca local |oauth_local_git_branch|, i ens hem d'assegurar que no hi ha canvis locals sense comitejar. En d'haver-n'hi, comitejar-los
primer avans de continuar amb el següent pas, recuperar els canvis del repositori

.. parsed-literal::

    git checkout master
    git pull
    git checkout |oauth_local_git_branch_c|
    git merge master

En actualitzacions simples, és poc probable que tinguem cap conflicte. En cas de tenir-ne
(per exemple si s'ha actualtizat els buildouts, o templates) resoldre els conflictes. En ambdós casos, comitejem els canvis del merge::

    git commit


Si hem de actualtizar configuració individual de la instància, aquest es el moment de fer-ho, i en acavat, comitejem els canvis de nou::

    git commit . -m "Local changes"


Tot seguit executem el buildout, deixem els permisos del sistema de fitxers en ordre,
i reiniciem el procés corresponent:

.. parsed-literal::

    ./bin/buildout -N -c osiris-only.cfg
    chown -R  |oauth_process_uid_c|:|oauth_process_uid_c|.
    /etc/init.d/osiris_{instance_name} restart



Actualitzar el utalk
--------------------

.. parsed-literal::

    ssh |utalk_ssh_user_c| @ |utalk_server_c|
    cd /var/utalk.upcnet.es
    git status


En aquest punt, hauriem de estar en la branca local ``finestrelles``, i ens hem d'assegurar que no hi ha canvis locals sense comitejar. En d'haver-n'hi, comitejar-los
primer avans de continuar amb el següent pas, recuperar els canvis del repositori

.. parsed-literal::

    git checkout master
    git pull
    git checkout finestrelles
    git merge master

En actualitzacions simples, és poc probable que tinguem cap conflicte. En cas de tenir-ne
(per exemple si s'ha actualtizat els buildouts, o templates) resoldre els conflictes. En ambdós casos, comitejem els canvis del merge::

    git commit


Si hem de actualtizar configuració individual de la instància, aquest es el moment de fer-ho, i en acavat, comitejem els canvis de nou::

    git commit . -m "Local changes"


Tot seguit executem el buildout, deixem els permisos del sistema de fitxers en ordre,

.. parsed-literal::

    ./bin/buildout -N -c talk-only.cfg
    chown -R  |max_process_uid_c|:|max_process_uid_c|.

Per últim reiniciem els processos que calgui segons el que haguem actualitzat des del
circus de http://finestrelles.upcnet.es:13001



Actualitzar el bigmax
---------------------

.. parsed-literal::

    ssh |max_ssh_user_c| @ |max_server_c|
    cd /var/bigmax
    git status


En aquest punt, hauriem de estar en la branca local  |max_local_git_branch|, i ens hem d'assegurar que no hi ha canvis locals sense comitejar. En d'haver-n'hi, comitejar-los
primer avans de continuar amb el següent pas, recuperar els canvis del repositori

.. parsed-literal::

    git checkout master
    git pull
    git checkout  |max_local_git_branch_c|
    git merge master

En actualitzacions simples, és poc probable que tinguem cap conflicte. En cas de tenir-ne
(per exemple si s'ha actualtizat els buildouts, o templates) resoldre els conflictes. En ambdós casos, comitejem els canvis del merge::

    git commit


Si hem de actualtizar configuració individual de la instància, aquest es el moment de fer-ho, i en acavat, comitejem els canvis de nou::

    git commit . -m "Local changes"


Tot seguit executem el buildout, deixem els permisos del sistema de fitxers en ordre,

.. parsed-literal::

    ./bin/buildout -N -c bigmax-only.cfg
    chown -R  |max_process_uid_c|:|max_process_uid_c|.
    /etc/init.d/bigmax restart
