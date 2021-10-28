.. module:: geoserver.ogcapi.coverages.missing
   :synopsis: What is missing on current implementation.

.. _geoserver.ogcapi.coverages.missing:

What is missing
===============

As reported at the beginning of this chapter, the OGC-API Coverages specification is still in draft state and the current GeoServer module is still an incomplete implementation of that specification.
The following resources and capabilities are missing:

* The  RangeType resource, describing the data values semantics, in other words, the bands, their data type and unit of measure.

* Range-subset support, allowing to retrieve only a subset of the bands available.

* Scaling support (``scale-size``, ``scale-factor`` and ``scale-axes``) allowing to re-scale the output coverage.

* JSON output for CIS 1.1 (only GeoTIFF, PNG, JPEG and GML are supported right now).

* Support for advanced capabilities such as reprojection and resampling. These will be included in dedicated extensions. Currently the CRS extension has a `minimal specification <https://github.com/opengeospatial/ogcapi-coverages/blob/master/extensions/crs.adoc>`_, providing a glimpse into how reprojection could work.

