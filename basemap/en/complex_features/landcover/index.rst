.. module:: hale.landcover
   :synopsis: Example showing how to publish land cover data according to the INSPIRE Data Specification on Land Cover.

.. _hale.landcover:

Land Cover example
==================

This module provides an example of how to setup a WFS service serving INSPIRE compliant Land Cover data. You will be guided through the whole process, step by step.

The full example project is available :download:`here <data/landcover_example.halez>`.

.. The same example project, but based on denormalized data, is available :download:`here <data/landcover_example_denormalized.halez>`.

The Land Cover data used in the example have been originally extracted from the `Geoscopio web portal <http://www502.regione.toscana.it/geoscopio/cartoteca.html>`_ by Regione Toscana (Italy) and are licensed under a Creative Commons Attribution - 4.0 Italia License. The data have been further elaborated in the context of the `LIFE+IMAGINE project <http://www.life-imagine.eu>`_.

Steps involved
--------------

.. toctree::
   :maxdepth: 1

   Creating a new HALE project <lcv_project>
   Importing source and target schemas <lcv_schemas>
   Defining the alignment <lcv_alignment>
   Configuring GeoServer <lcv_upload>
   Querying the data via WFS <lcv_query>
