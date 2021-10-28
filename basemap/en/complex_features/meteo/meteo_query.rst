.. module:: hale.meteo_query
.. _hale.meteo_query:

.. include:: <isonum.txt>

.. include:: ../common/query_intro.txt

Retrieve st:Station features by name
++++++++++++++++++++++++++++++++++++

This query retrieves all **st:Station** features whose **st:stationName** property begins with the string **'Rov'** (case sensitive).

The body of the **GetFeature** request is:

.. include:: st_getfeature_name.txt

Copy-paste the following command in the cURL shell to execute a **POST** request (reponse is saved in a file called **output.xml**)::

  curl -u admin:Geos -XPOST -H "Content-type: text/xml" -d @data/sample_requests/st_getfeature_name.xml http://localhost:8083/geoserver/ows > output.xml

The request should return 1 feature. This is a slightly simplified version of the response body:

.. include:: st_getfeature_name_response.txt

Filter st:Observation features by measured parameter value
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This query retrieves all **st:Observation** features that measured a wind speed higher than 100 Km/h.

The body of the **GetFeature** request is:

.. include:: st_getfeature_obs_wind.txt

Copy-paste the following command in the cURL shell to execute a **POST** request (reponse is saved in a file called **output.xml**)::

  curl -u admin:Geos -XPOST -H "Content-type: text/xml" -d @data/sample_requests/st_getfeature_obs_wind.xml http://localhost:8083/geoserver/ows > output.xml

The request should return 1 feature. This is a slightly simplified version of the response body:

.. include:: st_getfeature_obs_wind_response.txt

Filter st:Station features by contactMail parameter values
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This query retrieves all **st:Station** features that have at least one contact mail equals to *station1@stations1.org*.

The body of the **GetFeature** request is:

.. include:: st_getfeature_contactmail.txt

Copy-paste the following command in the cURL shell to execute a **POST** request (reponse is saved in a file called **output.xml**)::

  curl -u admin:Geos -XPOST -H "Content-type: text/xml" -d @data/sample_requests/st_getfeature_contact.xml http://localhost:8083/geoserver/ows > output.xml

The request should return 1 feature. This is a slightly simplified version of the response body:

.. include:: st_getfeature_contactmail_response.txt
