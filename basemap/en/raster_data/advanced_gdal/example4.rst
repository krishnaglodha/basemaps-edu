.. module:: geoserver.example4

.. _geoserver.example4:

Example n° 4: Serving a large number of satellite/aerial RGB GeoTiff with compression
-------------------------------------------------------------------------------------

While optimizing the Boulder aerial data set a large number of parameters have been added to the ``gdal_traslate``
and ``gdal_addo`` commands have been added without explanation, this section closes the gap by describing them.

When serving data in RGB format it is possible to compress the data efficiently and with good visual
results using TIFF internal JPEG compression. In other words, the file is a TIFF, but each tile and
overview is JPEG compressed to reduce space consumption. While serving GetMap requests the cost of
decompressing from JPEG is normally compensated by the lower amount of bytes to be moved from disk.

Let's have a look at the gdal_translate command used::

    gdal_translate -co "TILED=YES" -co "BLOCKXSIZE=512" -co "BLOCKYSIZE=512"
                   -co "COMPRESS=JPEG" -co "PHOTOMETRIC=YCBCR" -co "QUALITY=85"
                   aerial\13tde815295_200803_0x6000m_cl.tif
                   retiled\13tde815295_200803_0x6000m_cl.tif

The parameters are:

* ``COMPRESS=JPEG``: activates the JPEG compression with the default parameters, that is, RGB photometric interpretation and a quality of 75%
* ``PHOTOMETRIC=YCBCR``: switches the photometric interpretation to the yCbCr color space, which allows a significant further reduction in output size with minimal changes on the images (looking at the two images side by side, the yCbCr one may look a bit washed out compared to the RGB one). This color space is used by major satellite image providers (e.g., Google own tiles)
* ``QUALITY=85``: increases the quality level of the output, making the compression artifacts less visible bringing the result closer to a "visually lossless" situation

Calling the **gdalinfo** command on the generated image::

  gdalinfo retiled\13tde815295_200803_0x6000m_cl.tif

Results in::

    Driver: GTiff/GeoTIFF
    Files: retiled\13tde815295_200803_0x6000m_cl.tif
    Size is 2500, 2500
    Coordinate System is:
    PROJCS["NAD83 / UTM zone 13N",
        GEOGCS["NAD83",
            DATUM["North_American_Datum_1983",
                SPHEROID["GRS 1980",6378137,298.2572221010002,
                    AUTHORITY["EPSG","7019"]],
                AUTHORITY["EPSG","6269"]],
            PRIMEM["Greenwich",0],
            UNIT["degree",0.0174532925199433],
            AUTHORITY["EPSG","4269"]],
        PROJECTION["Transverse_Mercator"],
        PARAMETER["latitude_of_origin",0],
        PARAMETER["central_meridian",-105],
        PARAMETER["scale_factor",0.9996],
        PARAMETER["false_easting",500000],
        PARAMETER["false_northing",0],
        UNIT["metre",1,
            AUTHORITY["EPSG","9001"]],
        AUTHORITY["EPSG","26913"]]
    Origin = (481500.000000000000000,4431000.000000000000000)
    Pixel Size = (0.600000000000000,-0.600000000000000)
    Metadata:
      AREA_OR_POINT=Area
      TIFFTAG_RESOLUTIONUNIT=1 (unitless)
      TIFFTAG_SOFTWARE=IMAGINE TIFF Support
    Copyright 1991 - 1999 by ERDAS, Inc. All Rights Reserved
    @(#)$RCSfile: etif.c $ $Revision: 1.10.1.9.1.9.2.11 $ $Date: 2004/09/15 18:42:01EDT $
      TIFFTAG_XRESOLUTION=1
      TIFFTAG_YRESOLUTION=1
    Image Structure Metadata:
      COMPRESSION=YCbCr JPEG
      INTERLEAVE=PIXEL
      SOURCE_COLOR_SPACE=YCbCr
    Corner Coordinates:
    Upper Left  (  481500.000, 4431000.000) (105d13' 0.56"W, 40d 1'44.45"N)
    Lower Left  (  481500.000, 4429500.000) (105d13' 0.40"W, 40d 0'55.80"N)
    Upper Right (  483000.000, 4431000.000) (105d11'57.27"W, 40d 1'44.56"N)
    Lower Right (  483000.000, 4429500.000) (105d11'57.13"W, 40d 0'55.91"N)
    Center      (  482250.000, 4430250.000) (105d12'28.84"W, 40d 1'20.18"N)
    Band 1 Block=512x512 Type=Byte, ColorInterp=Red
    Band 2 Block=512x512 Type=Byte, ColorInterp=Green
    Band 3 Block=512x512 Type=Byte, ColorInterp=Blue

It is in particular important to check the **Image Structure Metadata** section , reporting a compression of "YCbCr JPEG"

Once this step is performed, overviews can be added to the file using **gdaladdo**. However, without further parameters the command would embed non compressed overviews in the file. In order to also compress the overviews the following command can be used::

    gdaladdo -r average --config COMPRESS_OVERVIEW JPEG
             --config PHOTOMETRIC_OVERVIEW YCBCR --config JPEG_QUALITY_OVERVIEW 85
             13tde815295_200803_0x6000m_cl.tif 2 4 8 16 32

The parameters are similar to the **gdal_translate** case, only using a slightly different syntax:

* ``COMPRESS_OVERVIEW JPEG``: activates the JPEG compression with the default parameters, that is, RGB photometric interpretation and a quality of 75%
* ``PHOTOMETRIC_OVERVIEW YCBCR``: switches the photometric interpretation to the yCbCr color space, which allows a significant further reduction in output size with minimal changes on the images (looking at the two images side by side, the yCbCr one may look a bit washed out compared to the RGB one). This color space is used by major satellite image providers (e.g., Google own tiles)
* ``JPEG_QUALITY_OVERVIEW 85``: increases the quality level of the output, making the compression artifacts less visible bringing the result closer to a "visually lossless" situation

Calling the **gdalinfo** command on the file again::

  gdalinfo retiled\13tde815295_200803_0x6000m_cl.tif

Results in::

    Driver: GTiff/GeoTIFF
    Files: 13tde815295_200803_0x6000m_cl.tif
    Size is 2500, 2500
    Coordinate System is:
    PROJCS["NAD83 / UTM zone 13N",
        GEOGCS["NAD83",
            DATUM["North_American_Datum_1983",
                SPHEROID["GRS 1980",6378137,298.2572221010002,
                    AUTHORITY["EPSG","7019"]],
                AUTHORITY["EPSG","6269"]],
            PRIMEM["Greenwich",0],
            UNIT["degree",0.0174532925199433],
            AUTHORITY["EPSG","4269"]],
        PROJECTION["Transverse_Mercator"],
        PARAMETER["latitude_of_origin",0],
        PARAMETER["central_meridian",-105],
        PARAMETER["scale_factor",0.9996],
        PARAMETER["false_easting",500000],
        PARAMETER["false_northing",0],
        UNIT["metre",1,
            AUTHORITY["EPSG","9001"]],
        AUTHORITY["EPSG","26913"]]
    Origin = (481500.000000000000000,4431000.000000000000000)
    Pixel Size = (0.600000000000000,-0.600000000000000)
    Metadata:
      AREA_OR_POINT=Area
      TIFFTAG_RESOLUTIONUNIT=1 (unitless)
      TIFFTAG_SOFTWARE=IMAGINE TIFF Support
    Copyright 1991 - 1999 by ERDAS, Inc. All Rights Reserved
    @(#)$RCSfile: etif.c $ $Revision: 1.10.1.9.1.9.2.11 $ $Date: 2004/09/15 18:42:01EDT $
      TIFFTAG_XRESOLUTION=1
      TIFFTAG_YRESOLUTION=1
    Image Structure Metadata:
      COMPRESSION=YCbCr JPEG
      INTERLEAVE=PIXEL
      SOURCE_COLOR_SPACE=YCbCr
    Corner Coordinates:
    Upper Left  (  481500.000, 4431000.000) (105d13' 0.56"W, 40d 1'44.45"N)
    Lower Left  (  481500.000, 4429500.000) (105d13' 0.40"W, 40d 0'55.80"N)
    Upper Right (  483000.000, 4431000.000) (105d11'57.27"W, 40d 1'44.56"N)
    Lower Right (  483000.000, 4429500.000) (105d11'57.13"W, 40d 0'55.91"N)
    Center      (  482250.000, 4430250.000) (105d12'28.84"W, 40d 1'20.18"N)
    Band 1 Block=512x512 Type=Byte, ColorInterp=Red
      Overviews: 1250x1250, 625x625, 313x313, 157x157, 79x79
    Band 2 Block=512x512 Type=Byte, ColorInterp=Green
      Overviews: 1250x1250, 625x625, 313x313, 157x157, 79x79
    Band 3 Block=512x512 Type=Byte, ColorInterp=Blue
      Overviews: 1250x1250, 625x625, 313x313, 157x157, 79x79

Notice that while the overview presence is clear, their compression does not show up in the file metadata.
