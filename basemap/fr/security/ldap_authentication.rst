.. module:: geoserver.ldap_authentication

.. _geoserver.ldap_authentication:


LDAP Authentication Provider
----------------------------

The LDAP Authentication Provider uses and LDAP service to authenticate users.
It can be configured to use LDAP only for authentication, or also for roles assignment (each LDAP group is a role).

To configure the provider you have to specify:
 * a name for the provider 
 * the LDAP server url, complete with the root DN (e.g. http://localhost:389/dc=maxcrc,dc=com)
 * the User lookup pattern: {0} should be used in place of the username value in the pattern (e.g. uid={0},ou=People)
 * use TLS (secure connection) or not to connect to the server
 * if using LDAP groups for authorization, groups bound to the LDAP user are used as GeoServer roles, in this case you have to configure also:
 
  * the Group search base (e.g. ou=Groups)
  * the Group search filter, a search pattern for locating the LDAP groups a user belongs to. This may contain two placeholder values: {0}, the full DN of the user, for example uid=bob,ou=people,dc=acme,dc=com {1}, the uid portion of the full DN, for example bob
  
 * if NOT using LDAP groups for authorization:
 
  * choose one of the available user/group service for that purpose
   
We will now add a new LDAP authentication provider, but first we need to add a new user/group service, that the provider will use:

   .. note:: You will need an LDAP server (e.g. OpenLDAP) to do this exercise, the LDAP server is not part of the training material. We assume that an LDAP server is installed on localhost, with a dc=maxcrc,dc=com root and a user with uid ldapuser and password ldapuser exists.

#. From the :guilabel:`Welcome` page click the :guilabel:`Users, Groups, Roles` link on the :guilabel:`Menu` Security section. 
   .. note:: You have to be logged in as Administrator in order to activate this function.
   
   .. figure:: img/usergroup1.png

#. Click the :guilabel:`Add new` in the :guilabel:`User Group Services` menu    

	- Insert ``ldapservice`` in the ``Name`` text field.
	- Select ``Weak PBE`` from ``Password encryption`` combo box.	
	- Select ``default`` from ``Password policy`` combo box.
	- Insert ``ldapservice.xml`` in the ``XML filename`` text field.

   .. figure:: img/ldapprov2.png  
   
#. Click the :guilabel:`Save` button. 

Now we are going to add a user to the newly added user/group service:

#. From the :guilabel:`Welcome` page click the :guilabel:`Users, Groups, Roles` link on the :guilabel:`Menu` Security section

#. Click on the :guilabel:`User/Groups` tab

#. Click on the :guilabel:`ldapservice` link and the user/groups form will appear

#. Click on the :guilabel:`edit` link to the right of the :guilabel:`ldapservice` link

#. Click on the :guilabel:`Users` tab

#. Click on the :guilabel:`Add new user` button

	- Insert ``ldapuser`` in the ``User name`` text field
	- Insert ``fake`` in the ``Password`` and ``Confirm Password`` text fields (a password is always required, also if it is not used for authentication)	
	- Select the :guilabel:`ADMIN` element in the :guilabel:`Available` list of the :guilabel:`Roles taken from active role service: default` menu
	- Click the :guilabel:`arrow right` button to add the element to the :guilabel:`Selected` list	

   .. figure:: img/usergroup3.png  
   
#. Click the :guilabel:`Save` button. 

Now we are ready to add the Authentication provider:   

#. From the :guilabel:`Welcome` page click the :guilabel:`Authentication` link on the :guilabel:`Menu` Security section. 

#. Click :guilabel:`Add new` in the :guilabel:`Authentication Providers` menu
 
#. Click :guilabel:`LDAP` in the :guilabel:`Authentication Providers` list
 
	- Insert ``testldap`` in the ``Name`` text field.
	- Insert ``ldap://localhost:389/dc=maxcrc,dc=com`` in the ``Server URL`` text field.
	- Insert ``uid={0},ou=People`` in the ``User lookup pattern`` text field.
	- Uncheck ``Use LDAP groups for authorization`` checkbox.		
	- Select ``ldapservice`` from ``User/Group service`` combo box.	

   .. figure:: img/ldapprov1.png  

#. Click the :guilabel:`Save` button.

#. From the :guilabel:`Welcome` page click the :guilabel:`Authentication` link on the :guilabel:`Menu` Security section. 

#. Select the :guilabel:`testldap` element in the :guilabel:`Available` list of the :guilabel:`Provider Chain` menu

#. Click the :guilabel:`arrow right` button to add the element to the :guilabel:`Selected` list

#. Click the :guilabel:`Save` button.

Now, we have activated a new Authentication provider, having a new administrator user, named ldaptest. To verify it:

#. Click the :guilabel:`Logout` button on the top right part of the page.

#. Isert ``ldaptest`` in the ``Username`` and password text fields on the top right part of the page.

#. Click the :guilabel:`Login` button on the top right part of the page.

You should be now logged in with the ldaptest user, with administrative rights.
