.. module:: basemap.imp-docker
   :synopsis: Exploring OSM Docker image for basemap OSM stack.

.. _basemap.imp-docker:


Understanding ImpOSM
----------------------

You can get the github link for it `here <https://github.com/geosolutions-it/docker-osm/tree/basemap>`_

This package allows us to take OSM data in *.pbf* format and push it to PostgreSQL. 

Current setup allows user to select the countries from Europe that they are interested in. 
Refer `countries list <https://download.geofabrik.de/europe.html>`_  here. By default data for **great-britain** will be loaded, to make changes, head over to `Makefile` and look for `imposm` build code, here then you can put all countries name.
e.g. To add albania, finland, Poland, Spain the code will be as follows
::
   	@cd build/docker-osm/docker-imposm3 && chmod +x countries.sh && ./countries.sh albania finland poland spain

This will download all required PBF file and wait for PostgreSQL connection.