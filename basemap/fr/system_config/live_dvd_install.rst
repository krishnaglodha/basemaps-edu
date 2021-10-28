.. module:: geoserver.live_dvd_install

.. _geoserver.live_dvd_install:


Installer le DVD sur le système
------------------------------------

Afin d'augmenter les prestations du système et, dans le cas que vous etes en train de faire marcher le DVD, pour garder votre travail pour une autre session plus tard, il pourrait etre utile d'*installer* le DVD sur le système. On peut le faire soit en installant le système operatif sur votre machine soit sur une machine virtuelle (VM) comme VMWare ou VirtualBox. 


   .. note::  A fin de bien travailler sans problèmes on **recommande** de  faire le ``GEOSERVER_DATA_DIR`` point dans un repertoire sur le persistent storage (voir la séction suivante)


#. Suivez les instructions dans la section :ref:`previous <geoserver.live_dvd_running>` pour permettre au DVD de marcher. 

   .. note::  Configurer un nouvel VM compatible avec Ubuntu et lui attacher le DVD, vous permettra aussi d'installer le groupe de travail sur un environnement virtual. 

#. Une fois obtenu l'écran d'initialisation du DVD comme montré en dessous, tapez **install** et appuyez sur ENTER

   .. figure:: img/dvd_install0.jpg
      :width: 600
	  
      LiveDVD boot écran

#. Choisissez la langue et cliquez sur :guilabel:`Continue` sur l'écran :guilabel:`Welcome`.

   .. figure:: img/dvd_install1.jpg
      :width: 600

      LiveDVD installation: choix de la langue

#. Choisissez le fuseau horaire et cliquez sur :guilabel:`Continue` sur l'écran :guilabel:`Where are you?` .

   .. figure:: img/dvd_install2.jpg
      :width: 600

      LiveDVD installation: choix du fuseau horaire

#. Choisissez le format de votre clavier et cliquez sur :guilabel:`Continue` sur l'écran :guilabel:`Keyboard`.

   .. figure:: img/dvd_install3.jpg
      :width: 600

      LiveDVD install: configuration du clavier

#. Sélectionnez la partition où vous voulez installer le système et cliquez :guilabel:`Forward` sur l'écran :guilabel:`Prepare disk space`.

   .. note::  On vous suggère de ne pas modifier la configuration ici proposée à moins d'etre un utilisateur expert et de savoir exactement ce que vous faites. 

   .. figure:: img/dvd_install4.jpg
      :width: 600

      LiveDVD install: Type d'installation

#. Sélectionnez le nom de l'utilisateur et le mot de passe et le hostname, et puis cliquez sur :guilabel:`Forward`.

   .. Attention:: Ce course envisage l'utilisation d'un ''nom utilisateur'' :**geosolutions** avec un ''mot de passe'' : **Geos**. Vous pouvez choisir n'importe quel ``hostname``.
   
   .. figure:: img/dvd_install5.jpg
      :width: 600

      LiveDVD install: selection ``qui etes vous?`` 

#. Controlez le resumé et cliquez sur :guilabel:`Install`.

   .. figure:: img/dvd_install6.jpg
      :width: 600

      LiveDVD install: pret à installer le resumé. 

#. Attendez pour que l'installation soit terminée et puis relancez le système. Rappelez-vous d'enlever le DVD du DVD-ROM avant de relancer l'ordinateur ou le VM.

   .. figure:: img/dvd_install7.jpg
      :width: 600

      LiveDVD install: progrès d'installation

   .. figure:: img/dvd_install8.jpg
      :width: 600

      LiveDVD install: installation terminée

   .. figure:: img/dvd_running.jpg
      :width: 600

      LiveDVD install: Système Operatif

   .. note::  Le non utilisateur et le mot de passe de OS à utiliser sont:
   
			  - username: `geosolutions`
			  - password: `Geos`
	  
   .. note::  Pour chaque application installée sur le système nous avons utilisé le nom utilisateur `geosolutions` avec le mot de passe `Geos`.

	A ce point là vous devrez avoir un environnement fonctionnel pour l'execution de GeoServer, les outils GIS et les clients sur votre données d'informations. Dans la séction  :ref:`next <geoserver.gs_prerequisites>` vérifiera l'installation en mettant en marche le serveur.
