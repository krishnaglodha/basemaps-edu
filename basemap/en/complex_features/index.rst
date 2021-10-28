.. HALE + GeoServer App-Schema documentation master file, created by
   sphinx-quickstart on Tue Aug 25 11:55:16 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Complex Features with GeoServer and HALE
========================================

This training module shows how  geospatial data stored in regular GeoServer data sources (Shapefiles, RDBMSs, etc...) can be mapped  to complex, object-oriented information models thanks to the **Application Schema** GeoServer extension and **HALE**.

The `Application Schema (App-Schema) <http://docs.geoserver.org/latest/en/user/data/app-schema/index.html>`_ extension provides support for Complex Features in GeoServer.

`HALE <http://community.esdi-humboldt.eu/projects/hale>`_ is a tool for defining and evaluating conceptual schema mappings. The goal of HALE is to allow domain experts to ensure logically and semantically consistent mappings and consequently transformed geodata. In the context of this tutorial, HALE will be used to generate the required App-Schema configuration through a visual, user-friendly interface, freeing the user from the burden of writing serveral XML files by hand (an arguably error prone approach).

What you will learn
-------------------

After a brief introduction, providing the essential theoretical background about features, application schemas, GML and their use in GeoServer, you will find several concrete examples that will walk you through the process of:

#. creating a new alignment project in **HALE**
#. visually defining a mapping between data stored in a PostGIS database and a complex GML application schema (such as the `INSPIRE Data Specification on Land Cover <http://inspire.ec.europa.eu/schemas/lcv/3.0/LandCoverVector.xsd>`_ and `GeoSciML <http://schemas.geosciml.org/geosciml/2.0/geosciml.xsd>`_)
#. finally, publishing your data as a WFS service through **GeoServer**.

Sections
--------

.. toctree::
   :maxdepth: 1

   intro/index
   installation/index
   meteo/index
   landcover/index
   geosciml/index
   faq/index
