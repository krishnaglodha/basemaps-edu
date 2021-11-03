.. module:: basemap.pg-docker
   :synopsis: Exploring PostgreSQL Docker image for basemap OSM stack.

.. _basemap.pg-docker:

Understanding Postgres Docker
-----------------------------

Image for Postgres to use in basemap OSM is already uploaded to docker hub, you can find it `here <https://hub.docker.com/repository/docker/krishnaglodha/postgis>`_ 

The Job of postgres in this solution is to hold the data coming from ImpOSM and project it back to Geoserver (master and slaves)