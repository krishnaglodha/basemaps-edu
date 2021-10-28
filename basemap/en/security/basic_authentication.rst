.. module:: geoserver.basic_authentication

.. _geoserver.basic_authentication:


Basic Authentication Provider
-----------------------------

The Basic Authentication Provider uses a :ref:`user/group service <geoserver.users_roles>` for authentication.
To configure the provider you have to specify a name for the provider and choose one of the available user/group service.

We are going to configure a new Authentication provider, but first we need to create a new user/group service.
Look at :ref:`User/group service <geoserver.xmlusergroup_services>` page to see how to create it.
When you have the service configured, go on and:

#. From the :guilabel:`Welcome` page click the :guilabel:`Authentication` link on the :guilabel:`Menu` Security section. 

#. Click :guilabel:`Add new` in the :guilabel:`Authentication Providers` menu
 
	- Insert ``testprovider`` in the ``Name`` text field.
	- Select ``testservice`` from ``User/Group service`` combo box.	

   .. figure:: img/authproviders2.png   

#. Click the :guilabel:`Save` button.

#. From the :guilabel:`Welcome` page click the :guilabel:`Authentication` link on the :guilabel:`Menu` Security section. 

#. Select the :guilabel:`testprovider` element in the :guilabel:`Available` list of the :guilabel:`Provider Chain` menu

   .. figure:: img/authproviders3.png

#. Click the :guilabel:`arrow right` button to add the element to the :guilabel:`Selected` list

#. Click the :guilabel:`Save` button.

Now, we have activated a new Authentication provider, having a new administrator user, named test. To verify it:

#. Click the :guilabel:`Logout` button on the top right part of the page.

#. Insert ``test`` in the ``Username`` and ``Password`` text fields on the top right part of the page.

#. Click the :guilabel:`Login` button on the top right part of the page.

You should be now logged in with the test user, with administrative rights.
