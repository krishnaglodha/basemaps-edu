.. module:: geoserver.python_gsconfig
   :synopsis: Python library for manipulating a GeoServer instance via the GeoServer RESTConfig API.

gsconfig
========

gsconfig is a python library for manipulating a GeoServer instance via the GeoServer RESTConfig API.

Installing
----------

For users: ``pip install gsconfig``

.. note:: You need Python and Setuptools installed on your system.

For developers: ``git clone https://github.com/boundlessgeo/gsconfig.git && cd gsconfig && python setup.py develop``
(`virtualenv <http://virtualenv.org/>`_ to taste.)


Connecting to GeoServer
-----------------------

.. note:: You need Python, Setuptools and 'gsconfig' package installed on your system before running the following exercises.

Once that gsconfig is installed you should be able to load it::

    $ python -m geoserver.catalog && echo "GeoServer loaded properly"

To connect to GeoServer, you can simply use the ``geoserver.catalog.Catalog`` constructor.
This example assumes that the default admin username and password are being used::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest")

That should work for connecting to a “default” GeoServer configuration, immediately after running the GeoServer installer for your platform or deploying the GeoServer WAR. 

If you are using other credentials (highly recommended for production) you can provide them to the constructor as well::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://example.com/geoserver/rest", username="root", password="t0ps3cr3t")

.. note:: Use "CTRL+Z" to exit the Python console.

For simplicity’s sake, other examples in this documentation will assume you’re working against a GeoServer installed locally using the default security settings.

Working with Layers
-------------------
Layers provide settings related only to the publishing of data. You can get a listing of all Layers configured in GeoServer::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    all_layers = cat.get_layers()

If you know a layer’s name you can also retrieve it directly::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    that_layer = cat.get_layer("roads")

Once you have a layer, you cannot manipulate its properties to change the configuration. This is possible only through the Layer Resource (see later).
However, you can see the Layer properties::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    that_layer = cat.get_layer("roads")
    print(that_layer.name)

Layers provide these settings:

* **enabled** is a boolean flag which may be set to **False** to stop serving a layer without deleting it. If this is set to **True** then the layer will be served.

* **default_style** is the style used in WMS requests when no style is specified by the client.

* **alternate_styles** is a list of other styles that should be advertised as suitable for use with the layer.

* **attribution_object** contains information regarding the name, logo, and link to more information about a layer’s provider.

Working with Resources
----------------------
Further settings, deemed more integral to the data, are available on the resource associated with the layer.
If you already have a layer object, you can get the corresponding resource easily::

    $ python
    
    resource = layer.resource

Alternatively, you can directly retrieve a list of all the resources::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    resources = cat.get_resources()

As with layers, you can retrieve a resource specifically by name::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    resource = cat.get_resource("roads")

With only one argument, get_resource will search all workspaces and stores for a resource with the given name.
However, it is possible to have multiple resources with a particular name.
If gsconfig detects that a request is ambiguous, it will raise geonode.catalog.AmbiguousRequestError rather than return a resource that might not be the one you had in mind. 
You can be more specific by specifying a workspace along with the name (although the name is always required.) 
For example, if you know that the **roads** resource is coming from a workspace named "sf" you can avoid an AmbiguousRequestError by telling the Catalog about it::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    resource = cat.get_resource("roads", workspace="sf")

It’s also possible to use a store or workspace object directly::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    workspace = cat.get_workspace("sf")
    resource = cat.get_resource("roads", workspace=workspace)

Once you have a resource, you can manipulate its properties to change the configuration.
However, no changes will actually be applied until you save it::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    resource = cat.get_resource("roads")
    resource.enabled = False
    
    # at this point that_layer is still published in GeoServer
    
    cat.save(resource)
      
    # now it is disabled

While FeatureTypes (vector resources) and Coverages (raster resources) each provide settings unique to their specific needs, there are some common settings as well:

* **title** is a string naming the layer in a human-friendly way. For example, it should be suitable for display in a layer listing GUI.

* **abstract** is a string describing the layer in more detail than the title.

* **keywords** is a list of short strings naming topics relevant to this dataset.

* **enabled** is a boolean flag which may be set to **False** to stop serving a resource without deleting it. If this is set to **True** then (assuming a corresponding enabled layer exists) the resource will be served.

* **native_bbox** is a list of strings indicating the bounding box of the dataset in its native projection (the projection used to actually store it in physical media.) The first four elements of this list will be the bounding box coordinates (in the order minx, maxx, miny, maxy) and the last element will either be an EPSG code for the projection (for example, “EPSG:4326”) or the WKT for a projection not defined in the EPSG database.

* **latlon_bbox** is a list of strings indicating the bounding box of the dataset in latitude/longitude coordinates. The first four elements of this list will be the bounding box coordinates (in the order minx, maxx, miny, maxy). The fifth element is optional and, if present, will always be “EPSG:4326”.

* **projection** is a string describing the projection GeoServer should advertise as the native one for the resource. The way this influences the actual values GeoServer will report for data from this resource are determined by the projection_policy.

* **projection_policy** is a string determining how GeoServer will interpret the projection setting. It may take three values:

    * FORCE_DECLARED: the data from the underlying store is assumed to be in the projection specified
    * FORCE_NATIVE: the projection setting is ignored and GeoServer will publish the projection as determined by inspecting the source data
    * REPROJECT: GeoServer will reproject the data in the underlying source to the one specified

    These are enumerated as constants in the geoserver.support package.

* **metadata_links** is a list of links to metadata about the resource annotated with a MIME type string and a string identifying the metadata standard.

Working with FeatureTypes (Vector Data)
---------------------------------------

.. warning:: For the exercise substitute ``%TRAINING_ROOT%`` (for Windows) references on code, with the path to the training on your local machine.

The code below allows you to create a FeatureType from a Shapefile::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    geosolutions = cat.get_workspace("geosolutions")
    import geoserver.util
    shapefile_plus_sidecars = geoserver.util.shapefile_and_friends("%TRAINING_ROOT%/data/user_data/states")
      
    # shapefile_and_friends should look on the filesystem to find a shapefile
    # and related files based on the base path passed in
    #
    # shapefile_plus_sidecars == {
    #    'shp': 'states.shp',
    #    'shx': 'states.shx',
    #    'prj': 'states.prj',
    #    'dbf': 'states.dbf'
    # }
    # 'data' is required (there may be a 'schema' alternative later, for creating empty featuretypes)
    # 'workspace' is optional (GeoServer's default workspace is used by... default)
    # 'name' is required
    
    ft = cat.create_featurestore("test", shapefile_plus_sidecars, geosolutions)

* **attributes** is a list of objects describing the names and types of the fields in the data set.

Working with Coverages (Raster Data)
------------------------------------

* **request_srs_list** is a list of strings defining the SRS’s that GeoServer should allow in requests against this coverage. Each SRS should be specified by its EPSG code.

* **response_srs_list** is a list of strings defining the SRS’s that GeoServer should use for responding to requests against this coverage. Each SRS should be specified by its EPSG code.

* **supported_formats** is a list of strings identifying the formats that GeoServer should use for encoding responses to requests against this Coverage. New formats may be added by GeoServer extensions, but in a default installation of GeoServer these format names are accepted:

    * ARCGRID
    * IMAGEMOSAIC
    * GTOPO30
    * GEOTIFF
    * GIF
    * PNG
    * JPEG
    * TIFF

Working with Styles
-------------------
Styles provide rules for determining how a data layer should be rendered as an image for viewing.
You can get a listing of all the styles configured in GeoServer::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    all_styles = cat.get_styles()

If you know a style’s name you can also retrieve it directly::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    cat.get_style("point")
    print(that_style)
        <geoserver.style.Style object at 0x0000000002B52BA8>
    print(that_style.name)
        point
    print(that_style.href)
        http://localhost:8083/geoserver/rest/styles/point.xml

Additionally, you can follow the links from a layer to the Styles that are associated with it::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    that_layer = cat.get_layer("roads")
    that_style = that_layer.default_style
    print(that_style.href)
        http://localhost:8083/geoserver/rest/styles/line.xml

Styles are a bit odd out of all the objects in gsconfig in that they have no writable properties.
Instead, they are simply a small decoration around style files in SLD format which can be added, deleted, or replaced in full.

To add a style, generate an SLD somehow (gsconfig does not provide any facilities for doing this).
Typically this will be saved to a file, for example railroad.sld.

.. warning:: For the exercise substitute ``%TRAINING_ROOT%`` references on code, with the path to the training on your local machine.

This code will then add the SLD file to GeoServer as a style available for WMS requests::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    f = open("%TRAINING_ROOT%/data/user_data/foss4g_mainrd.sld")
    cat.create_style("test_sld", f.read())

To replace an existing style, simply add another parameter named overwrite::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    f = open("%TRAINING_ROOT%/data/user_data/foss4g_mainrd.sld")
    cat.create_style("test_sld", f.read(), overwrite=True)

If you need to remove the style instead, it looks a bit different::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    style = cat.get_style("test_sld")
    cat.delete(style)

Working with LayerGroups
------------------------
A LayerGroup “packages up” several layers to make them more convenient to access together.
You can get a listing of all the layers configured in GeoServer::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    all_groups = cat.get_layergroups()

If you know a LayerGroup’s name you can also retrieve it directly::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    that_group = cat.get_layergroup("boulder")

.. note:: For a single LayerGroup you need to use ``get_layergroup`` instead of ``get_layergroups``

Once you have a LayerGroup, you can manipulate its properties to find out what Layers and Styles it uses::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    that_group = cat.get_layergroup("boulder")
    assert len(that_group.styles) == len(that_group.layers)

When working with LayerGroups it is important to ensure that the layers list and styles list have the same length before saving any changes.

.. note :: GeoServer also lets us read and set the bounding box for LayerGroups via the REST API but gsconfig doesn’t support this yet.

Working with Stores
-------------------
Resources in GeoServer are always contained within a **store**.
A Store’s configuration includes details of how to connect to some store of spatial data,
such as login credentials for a PostgreSQL server or the file path to a GeoTIFF file.
You can get a listing of all the stores configured in GeoServer::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    all_stores = cat.get_stores()

If you know a Store’s name you can also retrieve it directly::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    that_store = cat.get_store("storms")

Once you have a store, you can manipulate its properties to change the configuration. However, no changes will actually be applied until you save it::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    that_store = cat.get_store("storms")
    that_store.enabled = False
    
    # at this point that_store is still enabled in GeoServer
    
    cat.save(that_store)
    
    # now it is disabled
    
    that_store.enabled = True
    cat.save(that_store)
    
    # now it is enabled again

Deleting a ``store`` from the ``catalog`` requires to purge all the associated ``layers`` first. This can be done by doing something like this::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    layer = cat.get_layer("test")
    cat.delete(layer)
    cat.reload()
      
    st = cat.get_store("test")
    cat.delete(st)
    cat.reload()

Stores provide one common setting:

* **enabled** A Boolean flag which may be set to **False** to stop serving the resources (and corresponding layers) for a store without deleting them or the store. If this is set to **True** then the layers will be available.

Working with DataStores (Vector Data)
-------------------------------------

* **connection_parameters** a dict containing connection details. The keys used and interpretation of their values depends on the type of datastore involved. See examples for some sample usage, or cross-ref-with-geotools for details on how to identify the parameters for datastores not covered there.

Working with CoverageStores (Raster Data)
-----------------------------------------

* **url** A URL string (usually with the file: pseudo-protocol) identifying the raster file backing the CoverageStore.

* **type** A string identifying the format of the coverage file. While GeoServer extensions can add support for additional formats, the following are supported in a “vanilla” GeoServer installation:
    * Gtopo30, GeoTIFF, ArcGrid, WorldImage, ImageMosaic

Working with Workspaces
-----------------------
Workspaces provide a logical grouping to help administrators organize the data in a GeoServer instance.
You can get a listing of all the workspaces configured in GeoServer::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    all_workspaces = cat.get_workspaces()

If you know a workspace’s name you can also retrieve it directly::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    that_workspace = cat.get_workspace("geosolutions")

Working with ImageMosaics
-------------------------

There are some functionalities allowing to manage the ``ImageMosaic`` coverages. It is possible to create new ImageMosaics, add granules to them,
and also read the coverages metadata, modify the mosaic ``Dimensions`` and finally query the mosaic ``granules`` and list their properties.

The gsconfig methods map the "REST APIs for ImageMosaic". 

Create a new ImageMosaic Store and Layer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. warning:: For the exercise substitute ``%TRAINING_ROOT%`` references on code, with the path to the training on your local machine.

In order to create a new ImageMosaic layer, you can prepare a zip file containing the properties files for the mosaic configuration. 
Refer to the GeoTools ImageMosaic Plugin guide in order to get details on the mosaic configuration. 
The package contains an already configured zip file with two granules.
You need to update or remove the ``datastore.properties`` file before creating the mosaic otherwise you will get an exception::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    cat.create_imagemosaic("NOAAWW3_NCOMultiGrid_WIND_test", "%TRAINING_ROOT%/data/user_data/NOAAWW3_NCOMultiGrid_WIND_test.zip")

By default the ``cat.create_imagemosaic`` tries to configure the layer too. If you want to create the store only, you can specify the following parameter::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    cat.create_imagemosaic("NOAAWW3_NCOMultiGrid_WIND_test", "NOAAWW3_NCOMultiGrid_WIND_test.zip", "none")

In order to retrieve from the catalog the ImageMosaic coverage store you can do this::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    store = cat.get_store("NOAAWW3_NCOMultiGrid_WIND_test")

To delete an ImageMosaic store, you can follow the standard approach, by deleting the layers first.
.. note:: At this time you need to manually cleanup the data dir from the mosaic granules and, in case you used a DB datastore, you must also drop the mosaic tables::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    layer = cat.get_layer("NOAAWW3_NCOMultiGrid_WIND_test")
    cat.delete(layer)
    cat.reload()
    cat.delete(store)
    cat.reload()

Harvesting Granules to the ImageMosaic
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
It is possible to add more granules to the mosaic at runtime.
With the following method you can add granules already present on the machine local path::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    store = cat.get_store("NOAAWW3_NCOMultiGrid_WIND_test")
    cat.harvest_externalgranule("file://%TRAINING_ROOT%/data/user_data/NOAAWW3_NCOMultiGrid__WIND_000_20131001T000000.tif", store)

The method below allows to send granules remotely via POST to the ImageMosaic.
The granules will be uploaded and stored on the ImageMosaic index folder::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    store = cat.get_store("NOAAWW3_NCOMultiGrid_WIND_test")
    cat.harvest_uploadgranule("%TRAINING_ROOT%/data/user_data/NOAAWW3_NCOMultiGrid__WIND_000_20131002T000000.zip", store)

Updating the ImageMosaic
^^^^^^^^^^^^^^^^^^^^^^^^
The method below allows you the load and update the coverage metadata of the ImageMosaic.
You need to do this for every coverage of the ImageMosaic of course.::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    coverage = cat.get_resource_by_url("http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/NOAAWW3_NCOMultiGrid_WIND_test/coverages/NOAAWW3_NCOMultiGrid_WIND_test.xml")
    coverage.supported_formats = ['GEOTIFF']
    cat.save(coverage)

By default the ImageMosaic layer has not the coverage dimensions configured. It is possible using the coverage metadata to update and manage the coverage dimensions.
.. note:: Notice that the ``presentation`` parameters accepts only one among the following values {'LIST', 'DISCRETE_INTERVAL', 'CONTINUOUS_INTERVAL'}::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    from geoserver.support import DimensionInfo
    timeInfo = DimensionInfo("time", "true", "LIST", None, "ISO8601", None)
    coverage.metadata = ({'dirName':'NOAAWW3_NCOMultiGrid_WIND_test_NOAAWW3_NCOMultiGrid_WIND_test', 'time': timeInfo})
    cat.save(coverage)

Retrieve the ImageMosaic Granules
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Once the ImageMosaic has been configured, it is possible to read the coverages along with their granule schema and granule info::

    $ python
    
    from geoserver.catalog import Catalog
    cat = Catalog("http://localhost:8083/geoserver/rest", username="admin", password="Geos")
    store = cat.get_store("NOAAWW3_NCOMultiGrid_WIND_test")
    coverages = cat.mosaic_coverages(store)
    schema = cat.mosaic_coverage_schema(coverages['coverages']['coverage'][0]['name'], store)
    granules = cat.mosaic_granules(coverages['coverages']['coverage'][0]['name'], store)
    print(granules)

The granules details can be easily read by doing something like this::

    $ python
    
    granules['crs']['properties']['name']
    granules['features']
    granules['features'][0]['properties']['time']
    granules['features'][0]['properties']['location']
    granules['features'][0]['properties']['run']

When the mosaic grows up and starts having a huge set of granules, you may need to filter the granules query through a CQL filter on the coverage schema attributes::

    $ python
    
    granules = cat.mosaic_granules(coverages['coverages']['coverage'][0]['name'], store, "time >= '2013-10-01T03:00:00.000Z'")
    print(granules)
    
    granules = cat.mosaic_granules(coverages['coverages']['coverage'][0]['name'], store, "time >= '2013-10-01T03:00:00.000Z' AND run = 0")
    print(granules)
    
    granules = cat.mosaic_granules(coverages['coverages']['coverage'][0]['name'], store, "location LIKE '%20131002T000000.tif'")
    print(granules)
    
