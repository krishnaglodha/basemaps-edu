.. module:: hale.geosciml
   :synopsis: Example showing how to publish geologic data according to the GeoSciML v2.0 schemas.

.. _hale.geosciml:

GeoSciML example
==================

This module provides an example of how to setup a WFS service serving GeoSciML v 2.0 compliant data. You will be guided through the whole process, step by step.

The full example project is available :download:`here <data/geosciml_example.halez>`.

Steps involved
--------------

.. toctree::
   :maxdepth: 1

   Creating a new HALE project <gsml_project>
   Importing source and target schemas <gsml_schemas>
   Defining the alignment <gsml_alignment>
   Configuring GeoServer <gsml_upload>
   Querying the data <gsml_query>
