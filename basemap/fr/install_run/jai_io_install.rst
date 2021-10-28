.. module:: geoserver.jai_io_install

.. _geoserver.jai_io_install:


 Installer the native JAI and ImageIO
-------------------------------------

L'API `Java Advanced Imaging <http://www.oracle.com/technetwork/java/javase/tech/jai-142803.html>`_ (JAI) est une bibliothèque de manipulation d'image avancé construit par Sun.  GeoServer nécessite que JAI travaille avec des couvertures et les exploite pour la génération de la sortie WMS. Par défaut, GeoServer GeoServer est livré avec la versione pure de Java JAI, mais **l'installation de la version native JAI dans votre JDK / JRE augmentera les performances de traitement d'image de façon significative**.

En particulier, l'installation de la JAI native est important pour tous les traitements raster, qui est largement utilisé dans les deux WMS et WCS à redimensionner, découper et reproject rasters. Installation de la JAI native est également important pour toute les lectures et les écritures raster , qui affecte WMS et WCS. Enfin, natif JAI est très utile même s'il n'y a pas de données raster en jeu, comme WMS output encodage necessite  l'écriture PNG / GIF / JPEG images, qui sont eux-mêmes raster.
Extensions natives sont disponibles pour Windows (32 bit), Linux et Solaris (32 and 64 bit systems). Mac Os X 10.6 (probablement aussi 10.5 et 10.4) est livré avec des extensions natives JAI installés par défaut en `/System/Library/Java/Extensions/`. Malheureusement extensions ImageIO natives ne sont pas disponibles pour Mac Os X.

.. note:: Ces installateurs sont limités pour permettre l'ajout d'extensions natives à une seule version du JDK / JRE sur votre système.  Si des extensions natives sont nécessaires sur plusieurs versions,déballage manuellement les extensions sera nécessaire.
.. note:: Ces installateurs sont en mesure d'appliquer les extensions pour le JDK / JRE actuellement utilisé. Si des extensions natives sont nécessaires sur un autre JDK / JRE que celle qui est actuellement utilisée, il sera nécessaire de désinstaller l'actuel, puis exécutez le programme d'installation contre le reste de JDK / JRE.

Installer native JAI on Linux
````````````````````````````````

#. Aller à la page `JAI download page <http://download.java.net/media/jai/builds/release/1_1_3/>`_ et télécharger l'installeur Linux pour la version 1.1.3, en choisissant l'architecture appropriée:

   * `i586` for the 32 bit systems
   * `amd64` for the 64 bit ones (even if using Intel processors)

#. Copiez les fichiers dans le répertoire contenant le JDK / JRE, puis exécutez-il.  Par exemple, sur un système 32 bits Ubuntu:
  
    $ sudo cp jai-1_1_3-lib-linux-i586-jdk.bin /usr/lib/jvm/java-6-sun-1.6.0.32
    $ cd /usr/lib/jvm/java-6-sun-1.6.0.32
    $ sudo sh jai-1_1_3-lib-linux-i586-jdk.bin
    # accept license 
    $ sudo rm jai-1_1_3-lib-linux-i586-jdk.bin
  
#. Aller à la page `JAI Image I/O download page <http://download.java.net/media/jai-imageio/builds/release/1.1/>`_ et télécharger le programme d'installation pour la version Linux 1.1, en choisissant l'architecture appropriée:

   * `i586` for the 32 bit systems
   * `amd64` for the 64 bit ones (even if using Intel processors)

#. Copiez les fichiers dans le répertoire contenant le JDK / JRE, puis exécutez-il.  Si vous rencontrez des difficultés, vous pouvez avoir besoin d'exporter la variable d'environnement: ``_POSIX2_VERSION=199209``. Par exemple, sur un système Linux Ubuntu 32 bits ::
    $ sudo cp jai_imageio-1_1-lib-linux-i586-jdk.bin /usr/lib/jvm/java-6-sun-1.6.0.32
    $ cd /usr/lib/jvm/java-6-sun-1.6.0.32
    $ sudo su
    $ export _POSIX2_VERSION=199209
    $ sh jai_imageio-1_1-lib-linux-i586-jdk.bin
    # accept license
    $ rm ./jai_imageio-1_1-lib-linux-i586-jdk.bin
    $ exit

