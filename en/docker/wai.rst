.. module:: geoserver.wai

.. _geoserver.wai:


Web Administrator Interface
---------------------------


In this module we will get acquainted with the GeoServer 2.18.x Web Administrator Interface.

#. Navigate to the GeoServer `Welcome Page <http://localhost:8083/geoserver/>`_.
 
   .. figure:: img/wai1.png

      GeoServer welcome page
      
#. From the :guilabel:`Welcome` page, insert in the :guilabel:`Login section` the credentials user **admin** and password **Geos**.
         
   When you are logged on as administrator you can easily distinguish several sections:
   
   .. figure:: img/wai2.png

      Web Administration Interface sections
      
   - The left section is the navigation menu, it enables the admin to access all the different GeoServer configurations
   - The middle-top section gives quick access to the most common actions: list and edit layers, stores, and workspaces.
   - The middle-bottom section provides the admin some security advertisement.
   - The right section lists all the available services  
   
#. The most important administration sections are listed below, we will examine other sections later in this workshop:

   a) From the :guilabel:`Welcome` page, navigate to the :guilabel:`Server Status` page by clicking the link in the :guilabel:`About & Status` section.
   
      .. figure:: img/wai3.png

         Server Status sections
         
   b) From the :guilabel:`Welcome` page, navigate to the :guilabel:`GeoServer Logs` page by clicking the link in the :guilabel:`About & Status` section.
   
      .. figure:: img/wai4.png

         GeoServer Logs sections
   
   c) From the :guilabel:`Welcome` page, navigate to the :guilabel:`Caching Defaults` page by clicking the link in the :guilabel:`Tile Caching` section.
   
      .. figure:: img/wai5.png

         GeoWebCache settings sections

   d) From the :guilabel:`Welcome` page, navigate to the :guilabel:`Contact Information` page by clicking the link in the :guilabel:`About & Status` section.
   
      .. figure:: img/wai6.png
	  
         Contact Information sections

         From that section, you can fill in Contact Information details 
  

#. **IMPORTANT** - For all the links of this training to work correctly, you have to disable the URL parameters encryption under **Security** --> **Settings**.
   
   Unselect the **Encrypt web admin URL parameters** option and *Save*
   
   .. figure:: img/wai7.png
   
     Encrypt web admin URL parameters
   
