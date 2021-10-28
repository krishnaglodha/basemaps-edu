.. _geoserver.standalone_package:

Utiliser le standalone package
------------------------------

`Ici <http://geoserver.geo-solutions.it/downloads/training/new/>`__ vous allez trouver un ensemble de pacquets zip comprimés qui contiennent l'environnement complet pour GeoServer et son entrainement. Choisissez votre version (32 ou 64 bit) téléchargez et extrayez à votre hard drive local.  


   .. note::  Le paquet zip expédie une pre-installée vérsion JRE 6, en utilisant ce paquet vous acceptez implicitement cette `licence <http://www.oracle.com/technetwork/java/javase/terms/license/index.html>`__. Pour plusieurs détails réf. `ici <http://java.com/en/download/faq/distribution.xml>`__
   
Donwload
========

#. `Windows 32 Bit <http://geoserver.geo-solutions.it/downloads/training/new/Training_1.6_Win32.7z>`__
  
#. `Windows 64 Bit <http://geoserver.geo-solutions.it/downloads/training/new/Training_1.7_Win64.zip>`__

   
Structure et contenu du paquet
==============================

#.  **README.txt**

    Contient notes légaux

#.  **data**

    Contient tous les donnés utilisés pour le demo, plugins et les donnés GWC.
    
#.  **geoserver_data**
    The GeoServer data dir.
    
#.  **setenv.bat**

    Utilisé dans la plupart des textes pour préparer l'environnement, pour changer la version de Java utilisée, la geoserver data dir et la plupart des environnements, édite ce fichier.
	
    
#.  **jdk**

    Le Java embedded.

    
#.  **postgres**

    Le standalone PostgreSQL-9.2 + PostGIS-2.0 dossier d'installation (contient les 'data' directory avec les databases).
    
#.  **pgAdmin.bat**

    Déclence le PostgreSQL PgAdmin III admin interface.
    
#.  **postgis_setup.bat**

    Script optionnel pour re-créer le postgres database. (On ne l'utilise jamais normalement. Il utilise setenv.bat).
    
#.  **postgis_start.bat**

    Déclence le postgres server (Il utilise setenv.bat).
    
#.  **postgis_stop.bat**

    Arrete le postgres server (Il utilise setenv.bat).

#.  **tomcat-7.0.72**

    Contient deux différents exemples de tomcat.
    
#.  **tomcat_start_1.bat**

    Déclence le prémièr exemple de tomcat  (Il utilise setenv.bat)
    
#.  **tomcat_start_2.bat**

    Déclence le second exemple de tomcat  (Il utilise setenv.bat)
    
#.  **tomcat_stop_1.bat**

    Arrete le prémièr exemple de tomcat (Il utilise setenv.bat)
    
#.  **tomcat_stop_2.bat**

    Arrete le second exemple de tomcat (Il utilise setenv.bat)
    
#.  **udig**

    Contient l'installation de udig standalone
    
#.  **udig.bat**

    Starts uDig

Comme vous pouvez voire ce paquet contient tout ce dont vous pourrez avoir bésoin à fin d'avoir un environnement totalement fonctiponnel pour faire fonctionner GeoServer, outils SIG et les clients sur votre datasets
