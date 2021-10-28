.. module:: geoserver.ldap_authentication

.. _geoserver.ldap_authentication:


LDAP Authentication Provider
----------------------------

The LDAP Authentication Provider uses an LDAP service to authenticate users.

A simple LDAP server is included in this training. It can be configured to use LDAP only for authentication or also for roles assignment (each LDAP group is a role).

.. note:: The LDAP server must be started manually with the **ldap_start.bat** (win) or **ldap_start.sh** (linux) script located in the training root folder.

To configure the provider you have to specify:
 * a name for the provider 
 * the LDAP server url, complete with the root DN (e.g. http://localhost:10389/dc=acme,dc=org)
 * use TLS (secure connection) or not to connect to the server
 
Either one of:
 
 * the User lookup pattern: {0} should be used in place of the username value in the pattern (e.g. uid={0},ou=people)
 
or (for ActiveDirectory support)
 
 * the Filter used to lookup user: {0} should be used in place of the formatted username (see property below) or {1} for the username as is (e.g. (|(userPrincipalName={0})(sAMAccountName={1})))
 * the Format used for user login name: {0} should be used in place of the username value in the pattern (e.g. {0}@domain)
 
moreover you need to specify

 * if using LDAP groups for authorization: groups bound to the LDAP user are used as GeoServer roles, in this case you have to configure also: 
  * the Bind user before searching for groups (if anonymous searches are not allowed in the LDAP server)
  * the Group search base (e.g. ou=groups)
  * the Group search filter, a search pattern for locating the LDAP groups a user belongs to. This may contain two placeholder values: {0}, the full DN of the user, for example uid=bob,ou=people,dc=acme,dc=com {1}, the uid portion of the full DN, for example bob
  * the Group to use as ADMIN (LDAP group that works as ADMIN group for Geoserver)
  * the Group to use as GROUP_ADMIN (LDAP group that works as GROUP_ADMIN group for Geoserver)
 * if NOT using LDAP groups for authorization:
 
  * choose one of the available user/group service for that purpose
   
We will now add a new LDAP authentication provider.

   .. note:: You will need an LDAP server (e.g. OpenLDAP) to do this exercise. Use the one included, launching the **ldap_start.bat** (win) or **ldap_start.sh** (linux) script.

#. From the :guilabel:`Welcome` page click the :guilabel:`Authentication` link on the :guilabel:`Menu` Security section. 

#. Click :guilabel:`Add new` in the :guilabel:`Authentication Providers` menu
 
#. Click :guilabel:`LDAP` in the :guilabel:`Authentication Providers` list
 
	- Insert ``testldap`` in the ``Name`` text field.
	- Insert ``ldap://localhost:10389/dc=acme,dc=org`` in the ``Server URL`` text field.
	- Insert ``uid={0},ou=people`` in the ``User lookup pattern`` text field.
	- Leave the ``Use LDAP groups for authorization`` checkbox checked.
	- Insert ``ou=groups`` in the ``Group search base`` text field.
	- Insert ``member={0}`` in the ``Group search filter`` text field.
	- Insert ``admin`` in the ``Group to use as ADMIN`` text field.
	- Insert ``admin`` in the ``Group to use as GROUP_ADMIN`` text field.
	
   .. figure:: img/ldapprov1.png  

#. Click the :guilabel:`Save` button.

#. From the :guilabel:`Welcome` page click the :guilabel:`Authentication` link on the :guilabel:`Menu` Security section. 

#. Select the :guilabel:`testldap` element in the :guilabel:`Available` list of the :guilabel:`Provider Chain` menu

#. Click the :guilabel:`arrow right` button to add the element to the :guilabel:`Selected` list

#. Click the :guilabel:`Save` button.

Now, we have activated a new Authentication provider, having a new administrator user named bill. To verify it:

#. Click the :guilabel:`Logout` button on the top right part of the page.

#. Insert ``bill`` in the ``Username`` and ``hello`` in the ``Password`` text field on the top right part of the page.

#. Click the :guilabel:`Login` button on the top right part of the page.

You should be now logged in with the bill user, with some administrative rights (notice that not all menues are shown).

We also have some new NOT administrator user, for example bob. To verify it:

#. Click the :guilabel:`Logout` button on the top right part of the page.

#. Insert ``bob`` in the ``Username`` and ``secret`` in the ``Password`` text field on the top right part of the page.

#. Click the :guilabel:`Login` button on the top right part of the page.

You should be now logged in with the bob user, that does not have administrative rights.

LDAP Role Service Provider
----------------------------
An additional step permits to configure a role service to get GeoServer roles from the LDAP repository and allow access 
rights to be assigned to those roles.

We will now add one.

#. From the :guilabel:`Welcome` page click the :guilabel:`Users, Groups, and Roles` link on the :guilabel:`Menu` Security section. 

#. Click :guilabel:`Add new` in the :guilabel:`Role Services` section
 
#. Click :guilabel:`LDAP` in the :guilabel:`New Role Service` list
 
	- Insert ``testldaproles`` in the ``Name`` text field.
	- Insert ``ldap://localhost:10389/dc=acme,dc=org`` in the ``Server URL`` text field.
	- Insert ``ou=groups`` in the ``Group search base`` text field.
	- Insert ``member=uid={0},ou=people,dc=acme,dc=org`` in the ``Group user membership search filter`` text field.
	- Insert ``cn=*`` in the ``All groups search filter`` text field.

#. Click the :guilabel:`Save` button.

   .. figure:: img/ldapprov2.png  

Now that the role service is configured, we need to change it a bit to select groups for administrative purposes.

#. Click :guilabel:`testldaproles` in the :guilabel:`Role Services` list

#. Click :guilabel:`Roles` Tab to see the list of available roles, you should see ROLE_ADMIN and ROLE_USER

#. Click :guilabel:`Settings` Tab to go back to the Role Service configuration

#. Choose :guilabel:`ROLE_ADMIN` from the :guilabel:`Administrator role` combobox

#. Choose :guilabel:`ROLE_ADMIN` from the :guilabel:`Group administrator Role` combobox

#. Click the :guilabel:`Save` button.

Now, if you want, you can set this Role Service as the Active Role Service and use the LDAP roles
to constrain user(s) permissions.
