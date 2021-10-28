.. module:: geoserver.template

.. _geoserver.template:


Customizing KML Placemark with Templates
----------------------------------------

In KML, each entry in a data set is represented with a placemark. A placemark has a title and a description which is shown in a pop-up bubble when the placemark is clicked on. With templates placemark titles and descriptions can be easily customized. This section covers the task of creating custom title and description templates.

#. Open the following link to open the ``storm_obs`` layer in superoverlay mode::

	 http://localhost:8083/geoserver/wms/kml?layers=geosolutions:storm_obs&mode=superoverlay

.. warning:: If Google Earth will not automatically open, go to the system default download folder and double click on the downloaded kml file :file:`geosolutions-storm_obs.kml`. Zoom-in if you don't see the ``storm-obs`` layer.
   
#. In **Google Earth** click on a placemark.

   .. figure:: img/template1.png

      Viewing the default placemark description

#. On the file system navigate to the GeoServer data directory located at :file:`%TRAINING_ROOT%\\geoserver_data\\` on Windows or :file:`${{TRAINING_ROOT}}/geoserver_data/` on Linux.

.. warning:: You have to use slash ( :file:`/` ) instead of backslash ( :file:`\\` ) on Linux paths, when this one is the unique difference, the linux paths will not be described in this chapter.

#. In the :file:`workspaces\\geosolutions\\storms\\storm_obs` directory create a new filed named :file:`title.ftl`.

   .. figure:: img/template2.png

      Creating a title template

   .. note:: The :file:`title.ftl` is a template that will be used to render the title of a placemark

#. Open :file:`title.ftl` in the plain text editor of your choice (f.e. notepad) and enter the following content::

    Hurricane ${storm_name.value}

   .. note:: The above template content inserts the value of the **storm_name** attribute. When the template is rendered it will dynamically create a title that is the name of the hurricane. 

   .. figure:: img/template3.png

      Creating title template content

#. In Google Earth refresh the view by expanding ``stom_obs.kmz`` under "Temporary places", right clicking on ``geosolutions:storm_obs``, and clicking on the refresh menu item:


   .. figure:: img/refresh.png

      Forcing the refresh of a KML tree


#. Click on a placemark to see the new template in action:

   .. figure:: img/template4.png

      Viewing a custom placemark title


#. In the same directory as :file:`title.ftl` create a file named :file:`description.ftl`. Open the file and enter the following content:: 

     <img src="http://localhost:8083/geoserver/www/hurricane_warning.png"></img>
     <br>
     <br>
     On <b>${obs_datetime.value}</b> hurricane ${storm_name.value} was recorded to have a wind speed of <b>${wind.value}</b> mph.
     <br>
     <br>

   .. note:: The above template renders some HTML that contains a static image of a hurricane warning, as well as creates a paragraph of text describing in sentence form some information about the specific storm observation.

   .. figure:: img/template5.png

      Creating description template content

#. Save :file:`description.ftl` and refresh the view in Google Earth.

   .. figure:: img/template6.png

      Viewing a custom description placemark description

In this section templates were used to customize placemark visualization. In the next few sections the use of templates for other visualization purposes will be explored.
