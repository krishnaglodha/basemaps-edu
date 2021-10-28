.. module:: geoserver.limitations
   :synopsis: Know more about current limitations/assumptions

.. _geoserver.limitations:

Current limitations and assumptions
===========================================
In this section we are going to summarise the assumptions that GeoServer makes as well as the limitations the user needs to be aware of when serving multidimensional data.

ImageMosaic limitations
^^^^^^^^^^^^^^^^^^^^^^^
The ImageMosaic plugin has some limitations when it comes to serving multidimensional data:

  * *All the granules must share the same Coordinate Reference System, no reprojection is performed*.
  * *All the granules grid-to-world transformation must not bear rotation or skew*.  This means that at the moment only granules with a grid-to-world transformation that is a plan scale-and-translate are supported.
  * Mosaics of NetCDF-supported formats the dimension attributes should have the same name of the dimension attributes defined in the underlying H2 schema used by the NetCDF reader.
  
NetCDF limitations
^^^^^^^^^^^^^^^^^^
The NetCDF plugin has some limitations when parsing NetCDF files: 

  * Only WGS84 (EPSG:4326) CoordinateReferenceSystem is supported.
  * Only NetCDF files having dimensions following the COARDS convention are supported (custom, Time, Elevation, Lat, Lon).

WCS limitations
^^^^^^^^^^^^^^^
As far as the WCS protocol is concerned, the following limitations apply:

  * In case the specified output format doesn't support multidimensional output (e.g. GeoTiff), only a single granule will be returned using the default values combination illustrated previously. 
  * NetCDF output can be created for coverages served only by ImageMosaic and NetCDF.
  * When creating a NetCDF output, coverages should share the same BBOX (Lat/Lon coordinates are common for all granule) so they represent the same area of interest.
