.. _geoserver.styling:


Styling with SLD and CSS
========================

| This section introduces the concepts of the **Styled Layer Descriptor** (SLD) markup language.
| SLD is the styling engine used by GeoServer, and how all WMS portrayal is specified.

| In this section you will also use the `CSS extension module <http://docs.geoserver.org/latest/en/user/styling/css/index.html>`_ that allows you to build map styles using a compact, expressive styling language already well known to most web developers: **Cascading Style Sheets**.  
| The standard CSS language has been extended to allow for map filtering and managing all the details of a map production. The examples will have styles in SLD and CSS language.

In the official `GeoServer documentation <http://docs.geoserver.org/latest/en/user/styling/index.html#styling>`_ you can find useful references as the `SLD Cookbook <http://docs.geoserver.org/latest/en/user/styling/sld/cookbook/index.html>`_ and the `CSS Cookbook <http://docs.geoserver.org/latest/en/user/styling/css/cookbook/index.html>`_.

What you will learn
-------------------

In this section you will learn about:

.. toctree::
   :maxdepth: 1 

   Adding a style <add_style>
   Styling with CSS <css>
   Styling Vector data <styling_vector>
   Styling Raster data <styling_raster>
   Color compositing and blending <sld_composite-blend>