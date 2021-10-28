.. module:: geoserver.mosaic_basics
   :synopsis: ImageMosaic plugin Basic Concepts

.. _geoserver.mosaic_basics:

ImageMosaic Plugin Basic Concepts
------------------------------------

Background information
^^^^^^^^^^^^^^^^^^^^^^
ImageMosaic is a plugin that allows the creation of a mosaic from a number of georeferenced rasters files. 

The plugin can be used with a wide set of GeoServer supported formats, such as:

* **geotiff**
* other **raster formats** accompanied by a world file
* several `GDAL supported formats <http://docs.geoserver.org/latest/en/user/data/raster/gdal.html>`_ (in case the GDAL native libs are available)
* **NetCDF** and **GriB** files (check the latest section of this workshop for more info on current limitations).

Briefly, the ImageMosaic plugin is responsible for grouping together a set of similar raster data, which, from now on, we will call **granules**, and expose them as if they were a single, possibly multidimensional raster dataset. 
As an instance, ImageMosaic can be used to expose as a single dataset:

* a set of granules representing spatially adjacent acquisitions composing a wider view of a certain area on the earth.
  
  You may think about the case of Remote Sensing data where a satellite sensor acquires imagery (so called scenes) continuously by storing them into different files.
* a set of granules containing forecasts of a physical entity of the same geographical area at different times and/or different elevations or different dimensions (such as wind direction).
  
  You may think about some MetOc models providing air temperature, water temperature, wind speed data for different forecasts and different altitudes/depths.

Summarising, the ImageMosaic plugin is able to mosaic data spatially by taking into account multiple dimensions. As an instance it could be asked to create a raster by merging remote sensing data covering various adjacent areas on earth spanning over a certain time period. Internally it will keep track of the various information associated to each single granule through an index which is placed by default in a Shapefile although it can be placed in a spatial DBMS (more on this later).

ImageMosaic basic configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In GeoServer, an ImageMosaic is configured by specifying a Mosaic root directory. Usually such a folder contains a set of granules to be mosaicked as well as a minimal set of configuration files; the ImageMosaic plugin, once instantiated in GeoServer, parses the provided configuration files (that can be used to actually drive many aspect of the plugin functioning) and then starts looking for files/granules to be manage in a recursive way going down from the root directory. 
The ImageMosaic checks whether each granule it finds belongs to an already configured coverage or if it's a new one. 
Then it will update the internal granule catalogue accordingly in order to group together granules with similar characteristics such a number of bands, type of data, coordinate reference system and so on.

The set of granules composing the mosaic is saved along with the set of information pertaining to that granule (location, bounding box and more), into a persistent index which can be a Shapefile, a PostGIS database, an Oracle database or an H2 database.
Whether to store this information into a DBMS or in a simple Shapefile (*which is the default*) may be specified by configuring a proper set of information into a configuration file called *datastore.properties* file (more on this later).
Moreover, the set of dimensions available for the mosaic, the schema of the index, as well as the way to retrieve the values of the dimension values may be specified by properly configuring a set of auxiliary files.
More details about these configuration files will be introduced in the next chapter of the training.

.. warning:: The information in the following sections is very low-level since it details how the ImageMosaic plugin for GeoServer works internally.

ImageMosaic inner working
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
For each different coverage (measurement/variable) composing the store, the ImageMosaic uses a `RasterManager` instance which takes care of exposing metadata and dimensions information, as well as performing the read operation and giving access to the underlying granules source.

A `RasterManager` is internally made of a set of `DomainManagers`, one for each dimension associated to the coverage (time, elevation, customs). A `DomainManager` takes care of all the operations pertaining to dimensions, such as:

* exposing the read parameters to be used to access that dimension.
* exposing metadata names and values to represent the underlying dimension.
* setting up a proper filter to be used to query the granule source.

Finally, a special `SpatialDomainManager` is delegated to setup all the logics related to the 2D georeferencing context of the coverage, such as setting up the coordinate reference system, the geotransformation, and the bounding box.

When a read operation is requested to the ImageMosaic, it looks for the proper `RasterManager` to pass it the specified read parameters. The manager will then parse the parameters by setting up a `RasterLayerRequest` instance containing a representation of the request itself, which is used to produce a `RasterLayerResponse`.

The response will then process the request by producing back a GridCoverage. 

There are several steps involved when producing the response:

* Dealing with overviews.
* Setting up the proper math transformations and raster bounds computation to deal with raster data starting from a request expressed in real world coordinates.
* Handling filters on dimensions and queries.
* Retrieving the set of granules matching the requested parameters.
* Merging the retrieved granules into a single image or stacking them into a multi-band result.
* Applying postprocessing operation such as dealing with transparency and transforming back the image to the requested layout.

Accessing data
^^^^^^^^^^^^^^
Once the ImageMosaic has been configured, typical read operations are performed when accessing the granules by means of OGC service requests, such as WMS requests to get a portrayal of the raster data or WCS requests to get raw data from the underlying source. 

These aspects will be illustrated later on in this training, anyway it is worth to mention that once we request data, the ImageMosaic plugin will first access its own granule index (being it a Shapefile or a spatial DBMS) to gather information about which granules will be needed to satisfy the incoming request and then will use such information to perform the actual spatial mosaicking.

.. important:: It is worth to point out that the ImageMosaic plugin always work in a flat (2D) world, hence multidimensional data is treated as a (complex) set of 2D slices/granule describing a certain phenomenon in a specific geospatial area which many depend on other dimensions (wavelength, polarization, direction, time, elevation, depth, pressure and so on).

