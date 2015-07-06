Creació d'instàncies de ULearn Comunitats
=========================================

Per qualsevol error que pugui apareixer en la execució de les comandes, es pot continuar
des del punt en que ho ha deixat el gum de manera manual, seguint les :doc:`ulearn_manual`

Per crear una nova instància de ULearn Comunitats hem d'executar la seguent comanda:

    gum ulearn add instance {instance_name}

Aquesta comanda assumeix que hem creat un max amb el mateix nom ``{instance_name}``, i que està arrrencat i funcionant. Per lligar un ulearn comunitats amb un max amb nom diferent::

    gum ulearn add instance {instance_name} --max={max_instance_name}

on ``{max_instance_name}`` serà el nom de la instània amb la qual hem creat el max previament. Les instàncies de ULearn poden ser creades en més d'un servidor, per defecte es buscarà el primer mountpoint lliure i es crearà allà. Per especificar on es vol crear la instància::

    gum ulearn add instance {instance-name} --env={server}

on {server} és el dns d'un servidor de la llista que podem obtenir amb la comanda::

    gum ulearn list instances

Per defecte, no es crearàn instàncies en mountpoints que ja continguin una instància. Si es necessita ignorar aquesta premissa, es pot forçar la creació d'una instància en un mountpoint concret. Per fer-ho necessitarem saber el nom del ``{server}`` i el ``{num}`` de mountpoint, que podem obtenir amb la comanda anterior. Un cop conegudes les dades poderm crear la instància executant::

    gum ulearn add instance {instance-name} --env={server} --mpoint={num}


.. toctree::
   :maxdepth: 2

   ulearn_manual.rst
