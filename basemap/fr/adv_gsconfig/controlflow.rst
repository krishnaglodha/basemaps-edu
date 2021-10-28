.. geoserevr.controlflow:

Module Flux de contrôle 
=======================

Le module ``control-flow`` pour GeoServer permets à l'administrateur de controler la quantité de demandes simultanées réellement exécutées à l'intérieur du serveur.
Ce type de contrôle est utile pour plusieurs raisons:

*  *Performance*: des tests montrent que, des sources de données locales, le maximum débit dans les requetes `GetMap` est obtenu quand on permet de fonctionner en parallèle au plus 2 fois le numéro de CPU cores requetes.
*  *Resource control*: requetes comme `GetMap` peuvent utiliser une quantité importante de mémoire. Le :ref:`WMS request limits<geoserver.parameters>` permet de controler la quantité de memoire utilisée pour chaque requete, mais un ``OutOfMemoryError`` est encore possible s'il y a trop de requetes en parallèle. En controllant aussi la quantité de requetes en execution il est possible de limiter la quantité de mémoire utilisée au-dessous de la mémoire effectivement donnée à la Java Virtual Machine.
*  *Fairness*: un seul utilisateur ne devrait pas pouvoir submerger le serveur avec beaucoup de requetes, laissant les autres utilisateurs avec des tranches minuscules de la puissance de calcul global.

La méthode de contrôle de flux ne rejete pas normalement les demandes en excès, elle les laisse en attente en queue et les exécute en suite. Cependant, il est possible de configurer le module de façon qu'il rejete les requetes qui ont été en attente dans la queue trop longtemps.

Rule syntax référence
---------------------

Le courant implémentation du module Flux de contrôle lit ses règles dans le fichier de proprieté ``controlflow.properties`` situé dans le :ref:`GeoServer data directory <geoserver.gs_data_dir>`.

Total OWS request count
.......................

Le numéro global des requetes OWS en exécution en parallel peut être spécifiée avec::

   ows.global=<count>
   
Chaque requete en excés sera mise en queue et executé quand une autre requete complété laisse un slot d'exécution libre.

Controle Per requete 
.....................

On peut démander un type de contrôle par requête en utilisant la syntaxe suivante::

   ows.<service>[.<request>[.<outputFormat>]]=<count>

Où:

* ``<service>`` est le service OWS en question (au moment de l'écriture elle peut etre ``wms``, ``wfs``, ``wcs``)
* ``<request>``, optionnel, est le type de requete. Par example, pour le service ``wms``  elle peut etre ``GetMap``, ``GetFeatureInfo``, ``DescribeLayer``, ``GetLegendGraphics``, ``GetCapabilities``
* ``<outputFormat>``, optionnel, il est le format de sortie de la requête. Par example, pour la requete``wms`` ``GetMap`` il peut etre ``image/png``, ``image/gif`` etc.

Quelques examples::

  # ne permettez pas plus de 16 WCS requetes en parallèle
  ows.wcs=16
  # ne permettez pas plus de 8 GetMap requetes en parallèle
  ows.wms.getmap=8
  # ne permettez pas plus de 2 WFS GetFeature requetes avec le format de sortie Excel
  ows.wfs.getfeature.application/msexcel=2
  
Contrôle Per Utilisateur
.........................

Pour éviter q'un seul utilisateur fasse trop de réquetes en parallel::
  
  user=<count>
  
Où ``<count>`` est le maximum numéro de réquetes en parallèle q'un seul utilisateur peut executer en parallèle. Le mécanisme de suite de l'utilisateur est basé sur les cookie, alors cela fonctionnera très bien pour les navigateurs mais pas aussi bien pour les autres types de clients. Un mechanisme basé sur le IP n'est pas disponible actuellement, mais il aurait ses propres défauts aussi, car il limiterait tous les utilisateurs utilisant un seul routeur à ``<count>`` requetes (imaginez les éffects sur une grande administration publique).

Pause
.......

Une requete de Pause peut etre specifiée avec la syntaxe suivante::
 
   timeout=<seconds>
   
où ``<seconds>`` est le numéro de secondes pendant lesquels une requete peut rester an queue en attendant l'exécution. Si la demande n'entre pas dans l'exécution avant l'expiration du délai elle sera rejetée.

Un example complète 
-------------------

En supposant que le serveur que nous voulons protéjér ait 4 cores une exemple de configuration pourrait etre::

  # si une demande attend dans la queue pour plus de 60 secondes ça ne vaut pas l'exécution, 
  # le client aura probablement abandonné d'ici là
  timeout=60
  # ne permettre pas l'exécution de plus de 100 demandes totales en parallèle
  ows.global=100
  # ne permettre pas plus de 10 GetMap en parallèle 
  ows.wms.getmap=10
  # ne permettre pas plus de 4 sorties avec Excel depuis q'il est lié à sa mémoire 
  ows.wfs.getfeature.application/msexcel=4
  # ne permettre pas à un seul utilisateur d'effectuer plus de 6 requetes e parallèle
  # (puisque 6 est  le niveau de simultanéité de Firefox par défaut au moment de l'écriture)
  user=6
  

  