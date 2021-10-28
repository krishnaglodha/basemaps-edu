.. module:: hale.meteo
   :synopsis: Example showing how to publish observation data sampled by meteorological stations according to a user-defined GML application schema.

.. _hale.meteo:

Meteo Stations example
======================

This module provides an example of how to setup a WFS service serving meteorological data compliant with a user-defined GML application schema. You will be guided through the whole process, step by step.

.. The full example project is available :download:`here <data/landcover_example.halez>`.

Please note that this example uses fake data and a deliberately trivial GML application schema. Its intent is solely to demonstrate how to configure an App-Schema data store using HALE, with as few distractions as possible.

Steps involved
--------------

.. toctree::
   :maxdepth: 1

   Creating a new HALE project <meteo_project>
   Importing source and target schemas <meteo_schemas>
   Defining the alignment <meteo_alignment>
   Configuring GeoServer <meteo_upload>
   Querying the data via WFS <meteo_query>
   Exporting Complex features to an Isolated Workspace <meteo_isolated>
   Stored Query support <meteo_storedquery>
