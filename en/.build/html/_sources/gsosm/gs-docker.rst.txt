.. module:: basemap.gs-docker
   :synopsis: Exploring Geoserver Docker image for basemap OSM stack.

.. basemap.gs-docker:


Geoserver Docker image
--------------------
Geosolutions has created a geoserver docker image specifically for the basemap OSM.
You can get the github link for it `here <https://github.com/geosolutions-it/docker-geoserver/tree/basemap>`_ 

By default 2 instances of geoserver will be created. One for *master* and one for *slave*, user can add multiple slave geoservers and share data dir amongst them and then setup load balancer



Updating Makefile 
^^^^^^^^^^^^^^^^
Geoserver build code in Makefile has an ability to edit the version of geoserver and related plugins as well as add new plugins if needed
::
   	@cd build/docker-geoserver/ && sudo docker build -t geosolutionsit/geoserver:basemap --build-arg GEOSERVER_WEBAPP_SRC="https://sourceforge.net/projects/geoserver/files/GeoServer/2.19.1/geoserver-2.19.1-war.zip/download" --build-arg PLUG_IN_URLS="http://sourceforge.net/projects/geoserver/files/GeoServer/2.19.1/extensions/geoserver-2.19.1-control-flow-plugin.zip http://sourceforge.net/projects/geoserver/files/GeoServer/2.19.1/extensions/geoserver-2.19.1-libjpeg-turbo-plugin.zip http://sourceforge.net/projects/geoserver/files/GeoServer/2.19.1/extensions/geoserver-2.19.1-css-plugin.zip http://sourceforge.net/projects/geoserver/files/GeoServer/2.19.1/extensions/geoserver-2.19.1-monitor-plugin.zip http://sourceforge.net/projects/geoserver/files/GeoServer/2.19.1/extensions/geoserver-2.19.1-feature-pregeneralized-plugin.zip" .

here you can add new plugins by grabbing its url and adding to the list 

Onece the geoserver instances are up and running user can login to it using following credentials

.. list-table:: Geoserver access
   :widths: 25 25 25 25 
   :header-rows: 1

   * - Instance
     - URL
     - username
     - password
   * - master
     - <BASE_URL>/backoffice/
     - admin
     - dcGZS12Q4Vvh 
   * - slave
     - <BASE_URL>/geoserver/
     - admin
     - dcGZS12Q4Vvh 