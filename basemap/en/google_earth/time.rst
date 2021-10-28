.. module:: geoserver.time

.. _geoserver.time:


Generating a KML Time Series
----------------------------

When spatial data is combined with temporal information the visualization effects can be very interesting. Showing how objects change spatially over time is a powerful animation commonly referred to as a time series. This section covers how to use GeoServer templates in conjunction with Google Earth support for time to create a time series.

#. In the same directory that the :file:`title.ftl` and :file:`description.ftl` templates were created in the previous section, created a new template called :file:`time.ftl`.

   .. figure:: img/time1.png

      Creating a new time template

#. Open :file:`time.ftl` with a text editor and enter the following content::

     ${obs_datetime.value} 

   .. note:: Time templates work by obtaining a date from a **temporal** attribute. 

   .. figure:: img/time2.png

      Creating content for a time template.      

#. Save :file:`time.ftl`, delete the ``storm-obs`` layer and follow this link::

     http://localhost:8083/geoserver/wms/kml?layers=geosolutions:storm_obs&mode=download

   .. note:: This time we are using ``mode=download`` to have GE get the full data set, since we are going to see parts of it based on time as opposed to geographic area and importance

#. A time slider should appear at the top of the view port. 

   .. figure:: img/time3.png

      Viewing temporal data in Google Earth

#. Change the time interval in the time slider to about **1 month**, and adjust the **animation speed** to be slow.

   .. figure:: img/time4.png
    
      Adjusting time animation settings.

#. Click :guilabel:`OK` and start the animation clicking on the clock button on the top.

   .. figure:: img/time5.png

      Viewing the hurricane time series

At this point a temporal animation of the data set has been created with the simple use of templates. In the :ref:`next <geoserver.height>` section templates will be used yet again to add another aspect to the visualization.
