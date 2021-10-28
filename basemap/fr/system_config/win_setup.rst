.. module:: geoserver.win_setup

.. _geoserver.win_setup:


Windows automatic install
------------------------------------

Il est aussi possible d'installer sur un système Windows (32/64 bit) l'entier environnement sens la machine virtuelle.

#. Cliquer deux fois sur le fichier GS-Training-X.X.exe


   .. figure:: img/installer_001.jpg
      :width: 600
	  
      écran de bienvenus de Windows Installer

#. Accepter le License Agreement et bougez en avant jusq'à l'écran de 'Install Location'

   .. figure:: img/installer_002.jpg
      :width: 600
	  
      Installation de lécran de Localisation

   .. note::  Par défault l'installeur déployera le matériel de formation, Tomcat et GeoServer dans le répertoire des Program Files. Vous pouvez choisir n'importe quelle position. 
  

#. Choisissez le nom du répertoire 'start link'. Vous pouvez changer de nom si vous préférez une étiquette différente. 


   .. figure:: img/installer_004.jpg
      :width: 600
	  
      Commencer le nom du répertoire 
	  
	  Menu

#. Le prochain étape est l'installation de GDAL. La structure de GDAL est obligatoire pour l'exécution de quelq'un des exercises d'entrainement, donc si vous n'avez pas votre GDAL installation sur votre ordinateur, laissez qu'il en met en place une pour vous.

   .. figure:: img/installer_005.jpg
      :width: 600
	  

   .. figure:: img/installer_007.jpg
      :width: 600

      GDAL setup.

   .. note::  L'installation reconnaitra automatiquement votre OS bit-size (32/64).

#. Facultativement disposez les GDAL extensions pour ECW et MrSID aussi.

   .. figure:: img/installer_008.jpg
      :width: 600

      GDAL ECW extension.

   .. figure:: img/installer_009.jpg
      :width: 600

      GDAL MrSID extension.

#. L'installation de Postgresql et PostGIS est obligatoire. Si vous sautez ce passage, vous aurez bésoin d'installer manuellement le DB plus tard et d'actualiser les GeoServer stores pour q'ils se connectent à votre instance locale. 

   .. figure:: img/installer_010.jpg
      :width: 600

      PostgreSQL installation.

   .. figure:: img/installer_013.jpg
      :width: 600

      PostGIS installation.

   .. note::  Nous suggérons de ne pas modifiér la configuration ici proposée à mions d'étre un utilisateur expert et de savoir parfaitement ce que vous etes en train de faire.

   .. figure:: img/installer_015.jpg
      :width: 600

      PostGIS installation: license agreement.

   .. figure:: img/installer_016.jpg
      :width: 600

      PostGIS installation: template creation.

   .. figure:: img/installer_017.jpg
      :width: 600

      PostGIS installation: PostgreSQL installation setup.

   .. note::  Si le suivant avertisement se souleve procedéz en cliquant YES. 

   .. figure:: img/installer_018.jpg

      PostGIS installation: warnings.

   .. figure:: img/installer_019.jpg
      :width: 600

      PostGIS installation: warnings.

   .. note::  Attendez jusq'à ce que le message-guide se referme automatiquement par le procés d'installation.

   .. figure:: img/installer_020.jpg
      :width: 600

      PostGIS installation: DB configuration.
   
#. étapes finales d'installation.

   .. figure:: img/installer_021.jpg
      :width: 600

      Data deployment and final installation steps.

   .. Attention:: Lire attentivement le REAMDE. Vous ** devez** permettre droits d'écriture sur le dossier d'installation avant de commencer
   GeoServer.
   
   .. figure:: img/installer_022.jpg
      :width: 600

      README.

#. Controlez le Start Menu pour le repertoire d'entrainement.

   .. figure:: img/installer_023.jpg

      Training Start Folder Menu.

   .. note::  Le nom de l'utilisateur DB et le mot de passe à utiliser sont:
   
			  - username: `geosolutions`
			  - password: `Geos`

   .. note::  Le nom de l'utilisateur de GeoServer et le mot de passe à utiliser sont:
   
			  - username: `administrator`
			  - password: `Geos`
			  
#. Actualisez les droits finals des passages de l'installation . 

	Comme décrit en dessous **avant** de commencer à utiliser GeoServer, vous avez bésoin de permettre 'à tout le monde' d'obtenir l'accès au dossier d'installation  


   .. figure:: img/installer_024.jpg

   .. figure:: img/installer_025.jpg

   .. figure:: img/installer_026.jpg

#. Vous etes prets à commencer à utiliser GeoServer

   .. figure:: img/installer_027.jpg
