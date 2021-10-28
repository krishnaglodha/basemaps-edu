.. module:: geoserver.xmlusergroup_services

.. _geoserver.xmlusergroup_services:


XML Users and Group Services
---------------------------------

We will now see how to add a new service using xml files as a storage medium:

#. From the :guilabel:`Welcome` page click the :guilabel:`Users, Groups, Roles` link on the :guilabel:`Menu` Security section. 

   .. note:: You have to be logged in as Administrator in order to activate this function.
   
   .. figure:: img/usergroup1.png

#. Click the :guilabel:`Add new` in the :guilabel:`User Group Services` menu    

	- Insert ``testservice`` in the ``Name`` text field.
	- Select ``Weak PBE`` from ``Password encryption`` combo box.	
	- Select ``default`` from ``Password policy`` combo box.
	- Insert ``testservice.xml`` in the ``XML filename`` text field.

   .. figure:: img/usergroup2.png  
   
#. Click the :guilabel:`Save` button. 

Now we are going to add a user to the newly added user/group service:

#. From the :guilabel:`Welcome` page click the :guilabel:`Users, Groups, Roles` link on the :guilabel:`Menu` Security section

#. Click on the :guilabel:`User/Groups` tab
  
#. Click on the :guilabel:`testservice` link and the user/groups form will appear

   .. figure:: img/usergroup4.png

#. Click on the :guilabel:`edit` link to the right of the :guilabel:`testservice` link

#. Click on the :guilabel:`Users` tab

#. Click on the :guilabel:`Add new user` button

	- Insert ``test`` in the ``User name``, ``Password`` and ``Confirm Password`` text fields.	
	- Select the :guilabel:`ADMIN` element in the :guilabel:`Available` list of the :guilabel:`Roles taken from active role service: default` menu
	- Click the :guilabel:`arrow right` button to add the element to the :guilabel:`Selected` list	

   .. figure:: img/usergroup3.png  
   
#. Click the :guilabel:`Save` button. 

We will use this service in the :ref:`Basic Authentication <geoserver.basic_authentication>` section to create a new Authentication Provider.