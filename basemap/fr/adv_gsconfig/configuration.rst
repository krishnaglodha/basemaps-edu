.. geoserver.configuration:

Configuration du Moniteur
=========================

Plusieurs aspects de l'extension du moniteur configurables. Tous fichiers configuration
sont stockés dans le répertoire de données sous le répértoire ``monitoring`` ::

  <data_directory>
      monitoring/
          db.properties
          filter.properties
          hibernate.properties
          monitor.properties

On verra ci-dessous la fonction de ces fichiers.

.. _monitor_mode:

Moniteur Mode
--------------

l'extension du moniteur supporte différent "monitoring modes" qui controllent comment
les requetes des données sont is capturé et stockés. Actuellement, trois modes sont supportés:

  * **history** *(Default)* - Seules les informations historiques de requete sont disponibles. Aucune information est maintenue en directe.
  * **live** - Seules les informations de requete en directe sont maintenues.
  * **mixed** - Une combinaison de directe et historiques. Ce mode est expérimental.

Le mode est disposé dans le fichier ``monitor.properties``.

.. note:: Pour la Machine Virtuelle GeoServer on utilise le mode "live" .

Mode Historique 
^^^^^^^^^^^^^^^

Dans le Mode Historique les informations sur toutes les requetes persistent dans un base de données exterieur. Celui
ne fournit aucune information en temps réel. Ce mode est approprié au cas où
l'utilisateur est plus intéressé à analiser requetes historiques sur un temps donné.

Mode Temps Réel
^^^^^^^^^^^^^^^

Le Mode Temps Réel maintient seulement des informations de courte durée sur les demandes qui sont
en cours d'exécution. Il maintient également un peu de demandes récentes. Aucune 
base de données externe est utilisé avec ce mode et aucune information persiste
à long terme.

Ce mode est approprié au cas où l'utilisateur est interessé à ce que le serveur
est en train de faire en temps réel et s'il n'est pas intéressé aux requetes historiques.

Mode Mixte 
^^^^^^^^^^

Le Mode Mixte combine soit le Historique soit le Temps Réel, dans celui-là il maintient soit les  
informations en temps réel soit toutes les données des requetes à la base de données de surveillance. 
Cependant ce mode est experimental il a plus d'expenses que les autres deux, à cause du fait que 
le mode mixte doit effectuer de nombreuses transactions de base de données
au cours du cycle de vie d'une seule requête (pour avoir les informations en temps réel), 
pendant que le mode historique doit effectuer seulement une opération de base de données pour une 
requete.

Ce mode est approprié au cas où on veut en meme temps soit les requetes Historiques soit celles en Temps Réel. 
Ce mode est approprié aussi avec un serveur en cluster, dans lequel l'utilisateur est intéressé à visionner 
des informations sur plusieurs nœuds d'un cluster.

Base de données du Moniteur 
---------------------------

Par défaut des données de demande surveillés sont stockée dans une base de données intégrées H2 situées
dans le répértoire ``monitoring``. Cela peut etre changé en modifiant le fichier 
``db.properties``::

   # la configuration de défaut  
   est pour h2 
   driver=org.h2.Driver
   url=jdbc:h2:file:${GEOSERVER_DATA_DIR}/monitoring/monitoring

Par exemple pour stocker des données de requetes dans une base de données extérieure PostgreSQL::

   driver=org.postgresql.Driver 
   url=jdbc:postgresql://192.168.1.124:5432/monitoring
   username=bob
   password=foobar
   
Filtres des Requetes 
--------------------

Par défaut pas toutes les demandes sont surveillées. Cettes requetes comprennent les requetes de web admin ou les requetes moniteur HTTP API. Ces exclusions sont configurées dans le fichier ``filter.properties``:: 

   /rest/monitor/**
   /web/** 

Ces filtres de défaut peuvent être modifiés ou prolongés pour filtrer plusieurs types de
requetes. Par example pour filtrer toute requete WFS::

   /wfs

Comment déterminer le trajet du filtre
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Les contenus de ``filter.properties`` sont une série de modèles ant-style qui 
sont appliés au *path* de la requete. Considérons la requête suivante::

   http://localhost:8083/geoserver/wms?request=getcapabilities

Le trajet de la requete précèdente est ``/wms``. Dans la requete suivante::

   http://localhost:8083/geoserver/rest/workspaces/topp/datastores.xml

Le trajet est ``/rest/workspaces/topp/datastores.xml``.

En géneral, le trajet utilisé dans les filtres comprend la portion de URL
après ``/geoserver`` (including the preceding ``/``) et avant la chaîne de requête ``?``:: 

   http://<host>:<port>/geoserver/<path>?<queryString>

.. note::  Pour plus d'informations sur le modèle correspondant le ant-style, voir le `Apache Ant manual <http://ant.apache.org/manual/dirtasks.html>`_.
   
#. Ouvrez le fichier ``filter.properties`` situé en ${GEOSERVER_DATA_DIR}/monitoring et ajoutez le réglage suivant::

    /wms
    
#. Allez à la carte `Map Preview <http://localhost:8083/geoserver/web/?wicket:bookmarkablePage=:org.geoserver.web.demo.MapPreviewPage>`_ et ouvrez la couche `geosolutions:ccounties` clicquant sur le lien ``OpenLayer``.

#. Zoomez plusieur fois la carte.

#. Utilisez aussi l'avant-première GML pour la couche dite

#. Naviguez à la séction `Monitor/Reports <http://localhost:8083/geoserver/web/?wicket:bookmarkablePage=:org.geoserver.monitor.web.ReportPage>`_ 

#. cliquez sur ``OWS Request Summary`` pour montrer le graphique détaillé comme celui qui suit:

   .. figure:: img/monitor1.png
