.. module:: apache_http

.. _apache_http:


Load Balancer Setup with Apache HTTP
====================================

Here you'll find a quick setup to configure a proxy with load balancer to distribute requests between multiple instances of GeoServer each one running in a different Tomcat process.

  .. warning:: The following steps are OS specific as they involve creating scripts and setting environmental variables to run Tomcat. As such we will differentiate instructions between different OS.

Windows
+++++++

Into the provided package you'll find the **apache_start.bat** script which sets specific environment variables and starts the httpd server as standalone program.
Closing the window will shutdown the service.

The **apache_start.bat** script is used to setup the APACHE HTTPD settings and to startup the server::

    set TRAINING_ROOT=%~dp0

    set HTTPD_HOME=%TRAINING_ROOT%\Apache2.2

    set HTTPD_PORT=88

    call %ROOT%\setenv.bat

    set EXECUTABLE=%HTTPD_HOME%\bin\httpd.exe
    call %EXECUTABLE%

In the above script we setup some useful environment variables which will be used into the other configurations:

    #.	the HTTPD_HOME (set to %TRAINING_ROOT%/Apache2.2)
    #.	the HTTPD_PORT (set to 88)

.. note:: The default Apache port is 80. We are using port 88 to avoid any overlap.

Using the above script you may access to the Apache HTTPD server using a browser pointing to the URL::

    http://localhost:88

To setup the Load Balancer we still have to configure httpd to proxy the GeoServer tomcat instances.

HTTPD setup
***********

The main configuration file is located here::

    %HTTPD_HOME%/conf/httpd.conf

Edit that file to add global changes to the httpd server.

F.e. we have added the **HTTPD_HOME** prefix in front all the relative paths and set the server root and port as following::

    ServerRoot "${HTTPD_HOME}"

    #
    # Listen: Allows you to bind Apache to specific IP addresses and/or
    # ports, instead of the default. See also the <VirtualHost>
    # directive.
    #
    # Change this to Listen on specific IP addresses as shown below to 
    # prevent Apache from glomming onto all bound IP addresses.
    #
    #Listen 12.34.56.78:80
    Listen 0.0.0.0:${HTTPD_PORT}

Be also sure to uncomment the extra configuration inclusions::

    # Virtual hosts
    Include conf/extra/httpd-vhosts.conf

.. note:: In the distributed package you'll find this file under %HTTPD_HOME%/conf/extra/http-vhosts.conf

Which may result as following::


    <VirtualHost *:${HTTPD_PORT}>
      LoadModule proxy_ajp_module modules/mod_proxy_ajp.so
      LoadModule proxy_module modules/mod_proxy.so
      LoadModule proxy_balancer_module modules/mod_proxy_balancer.so
      LoadModule proxy_connect_module modules/mod_proxy_connect.so
      LoadModule proxy_ftp_module modules/mod_proxy_ftp.so
      LoadModule proxy_http_module modules/mod_proxy_http.so
      LoadModule reqtimeout_module modules/mod_reqtimeout.so

      <IfModule mod_proxy_ajp.c>
	  ProxyRequests Off
	  ProxyTimeout 300
	  ProxyPreserveHost On
	  ProxyVia On

	  <Proxy balancer://cluster>
	    BalancerMember ajp://localhost:8009 route=route1 timeout=20
	    BalancerMember ajp://localhost:8010 route=route2 timeout=20
	    ProxySet lbmethod=bybusyness
	  </Proxy>
	  <Location /geoserver>
	    Order allow,deny
	    Allow from all
	    ProxyPass balancer://cluster/geoserver stickysession=JSESSIONID
	  </Location>
  
      </IfModule>
    </VirtualHost>

.. note:: in the above configuration we consider the HTTPD_PORT environment variable set to an valid integer port number.

The used proxy parameters are:
  #. `ProxyRequests <http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#proxyrequests>`_
      This allows or prevents Apache from functioning as a forward proxy server. (Setting ProxyRequests to Off does not disable use of the ProxyPass directive.)
      
  #. `ProxyVia <http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#proxyvia>`_
      This directive controls the use of the Via: HTTP header by the proxy. We don't need it here, but it does not hurt either.
  
  #. `ProxyTimeout <http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#proxytimeout>`_
      This directive allows a user to specify a timeout on proxy requests. This is useful when you have a slow/buggy appserver which hangs, and you would rather just return a timeout and fail gracefully instead of waiting however long it takes the server to return.

  #. `ProxyPreserveHost <http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#proxypreservehost>`_
      When enabled, this option will pass the Host: line from the incoming request to the proxied host, instead of the hostname specified in the ProxyPass line.
  
  #. `ProxySet <http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#proxyset>`_
      This directive is used as an alternate method of setting any of the parameters available to Proxy balancers and workers normally done via the ProxyPass directive

The Proxy node contains all the BalancerMember of the cluster, in this configuration we configured 2 different instances of tomcat using the ajp connectors (on ports 8009, 8010).
It contains some other parameters:

  #. `Order <http://httpd.apache.org/docs/2.2/mod/mod_authz_host.html#order>`_ 
      The Order directive, along with the Allow and Deny directives, controls a three-pass access control system. The first pass processes either all Allow or all Deny directives, as specified by the Order directive.
      The second pass parses the rest of the directives (Deny or Allow).
      The third pass applies to all requests which do not match either of the first two.
  #. `Allow <http://httpd.apache.org/docs/2.2/mod/mod_authz_host.html#allow>`_
      The Allow directive affects which hosts can access an area of the server.
      Access can be controlled by hostname, IP address, IP address range, or by other characteristics of the client request captured in environment variables.

  #. `ProxyPass <http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#proxypass>`_
      This directive allows remote servers to be mapped into the space of the local server;
      the local server does not act as a proxy in the conventional sense, but appears to be a mirror of the remote server.
      The local server is often called a reverse proxy or gateway.
      The path is the name of a local virtual path;
      url is a partial URL for the remote server and cannot include a query string.

  #. `ProxyPassReverse <http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#proxypassreverse>`_
      This directive lets Apache adjust the URL in the Location, Content-Location and URI headers on HTTP redirect responses.
      This is essential when Apache is used as a reverse proxy (or gateway) to avoid by-passing the reverse proxy because of HTTP redirects on the backend servers which stay behind the reverse proxy


  #. `stickysession <https://httpd.apache.org/docs/2.2/mod/mod_proxy_balancer.html#stickyness>`_
      This directive is required for the user to be able to interact with GeoServer's UI. When a session is created for the user (e.g. as a result of the user logging into GeoServer's UI) all subsequent requests from the user will be routed through the same route instead of being balanced to both instances. This is required to properly interact with the UI.
       
.. warning:: When a session is created (e.g. you used the GeoServer GUI or the integrated GWC GUI) a cookie is set in your browser to transport the Session ID (it uses the key JSESSIONID). To observe the behaviour where incoming requests are being balanced to all GeoServer instances you need to either clear your browser cookies or open an `incognito mode <https://en.wikipedia.org/wiki/Privacy_mode>`_ window in the browser. Normally users going through the OGC services will not have a session id.

:ref:`For the tomcat connectors configuration click here <tomcat>`.

Each BalancerMember points to a tomcat instance using the ajp protocol (through its route: see below)::

    <Proxy balancer://cluster>
               BalancerMember ajp://localhost:8009 route=route1 timeout=20
               BalancerMember ajp://localhost:8010 route=route2 timeout=20
               ProxySet lbmethod=bybusyness
    </Proxy>

.. note:: The traffic is routed bybusyness but you may chose a different algorithm (see Proxy doc).

If you want to use the http connector you may change the proxy configuration pointing to those connectors::

    <Proxy balancer://cluster>
               BalancerMember http://localhost:8083
               BalancerMember http://localhost:8084
               ProxySet lbmethod=bybusyness
    </Proxy>

The incoming requests is redirect from the proxy to the load balancer by::

    <Location /geoserver>
	  Order allow,deny
	  Allow from all
	  ProxyPass balancer://cluster/geoserver stickysession=JSESSIONID
    </Location>

Each request to::

    http://localhost:88/geoserver

is now redirected to one of your tomcat instances depending on the incoming traffic and accordingly to the chosen lbmethod.
You are able to check the status of each Balancer Member using your browser (protected by authentication).

Linux
+++++

.. warning:: The following instructions are distribution-dependant and they have been tested on Ubuntu 12.04 that is the OS of the linux training environment. Although they should be valid on all the Debian-based distributions some changes may are required on other Debian or Ubuntu versions. For the Linux Red Hat-based distributions the apache package is called **httpd** instead of apache2.

Install the **apache webserver** on the linux training environment running the command::

    sudo apt-get install apache2

The apt-get installation process will install Apache 2.2.22 on your system as a system service.

start the apache2 daemon::
    
	sudo service apache2 start

Stop you apache2 daemon::

	sudo service apache2 stop

Reload the configuration files without stopping the service::

	sudo service apache2 reload

.. note:: For some configurations a reload is not enough and a restart is required, see the `official apache documentation <http://httpd.apache.org/docs/2.2/>`_ for more info

The main apache modules configuration directory is ``/etc/apache2/mods-enabled/``

To configure the apache2 proxy you have to configure the tomcat connector (see the previous chapter) and the webserver configuration.

First load all the apache2 needed modules that are not enabled by default::

    sudo cp /etc/apache2/mods-available/proxy.load /etc/apache2/mods-enabled
    sudo cp /etc/apache2/mods-available/proxy_ajp.load /etc/apache2/mods-enabled
    sudo cp /etc/apache2/mods-available/proxy_balancer.load /etc/apache2/mods-enabled


Then create a new configuration file under the apache modules configuration directory::

    sudo nano /etc/apache2/mods-enabled/proxy_balancer.conf

Edit it as follows::

    ProxyRequests Off
    ProxyTimeout 300
    ProxyPreserveHost On
    ProxyVia On
    <Proxy balancer://cluster>
        BalancerMember ajp://localhost:8009 route=route1 timeout=20
        BalancerMember ajp://localhost:8010 route=route2 timeout=20
        ProxySet lbmethod=bybusyness
    </Proxy>
    <Location /geoserver>
        Order allow,deny
        Allow from all
        ProxyPass balancer://cluster/geoserver stickysession=JSESSIONID
    </Location>

Save the file (``ctrl-o`` using nano as suggested) and restart apache as seen before (a configuration reload in not enough for this step).

Open the browser and access to the url::

    http://localhost/geoserver

The geoserver is now accessed through the apache2 webserver and the requests are balanced between the 2 geoserver instances available.

	
Further optional configurations
+++++++++++++++++++++++++++++++

Balancer manager
----------------

The following section is optional (not configured in the Linux/Windows packages). 
This is useful to check the configuration using your browser::

    <Location /balancer-manager>
       SetHandler balancer-manager
       AuthType basic
       AuthName "My_auth_name"
       AuthUserFile "/etc/httpd/passwd/passwords"
       # Anonymous *
       Require valid-user
    </Location>


.. note:: Here we also have set a basic authentication to access the basic auth

Point your browser to http://localhost:88/balancer-manager to see the manager.


Basic authentication
--------------------

To configure a basic authentication account on Linux proceed as following.

Create a password file::

    mkdir /etc/httpd/passwd
    touch /etc/httpd/passwd/passwords

Create the user::

    htpasswd -c /etc/httpd/passwd/passwords USER
    Password: PASSWORD

