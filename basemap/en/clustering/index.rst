.. GeoServer Advanced Training Modules

Clustering GeoServer
====================
Clustering GeoServer can be requested in order to achieve either an Highly Available set up or in order to achieve more scalability.
Regardless of the reason why intend to create a clustered deployment for GeoServer there are a few limitations that must be taken into account and where possible worked around.

In the remaining part of this section we will provide the attendee with information to gain a basic understanding of Clustering then we will dwelve into the various possibilities for clustering GeoServer and GeoWebCache discussing pros and cons of each approach.


Introduction to Clustering
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. toctree::
   :maxdepth: 2

   clustering/introduction
   
Basic (Passive) GeoServer Clustering
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. toctree::
   :maxdepth: 2

   clustering/passive/passive


Advanced (Active) GeoServer Clustering 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   
.. toctree::
   :maxdepth: 3
   
   clustering/active/index
   
Common load-balancing options for GeoServer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. toctree::
   :maxdepth: 1
   
   load_balancing/tomcat
   load_balancing/apache_http
   load_balancing/haproxy
   load_balancing/microsoft_IIS
   
   
   
