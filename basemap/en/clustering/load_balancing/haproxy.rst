.. module:: haproxy

.. _haproxy:

Load Balancer Setup with HAProxy
--------------------------------

.. warning::

    For all the links of this training to work correctly, you have to disable the URL parameters encryption under **Security** --> **Settings**.  
    
    Unselect the **Encrypt web admin URL parameters** option and *Save*

To install the HAProxy on an ubuntu server use the apt-get command::

      sudo apt-get install haproxy

We need to enable HAProxy to be started by the init script::

      sudo nano /etc/default/haproxy

Set the ENABLED option to 1::

      ENABLED=1

To check if this change is done properly execute the init script of HAProxy without any parameters.
You should see the following::

      sudo service haproxy
      Usage: /etc/init.d/haproxy {start|stop|reload|restart|status}

Now create and edit a new configuration file::

      sudo nano /etc/haproxy/haproxy.cfg

In the proposed configuration we provide a single frontend called (http-in) on the port 88 which will proxy 2 different backends:
    #. `geoserver <http://localhost:88/geoserver>`_ Is the default backend and will serve requests using a roundrobin algorithm to balance the **OGC** requests between all the available tomcat instances.
    #. `geoserver_adm <http://localhost:88/geoserver_adm>`_ Is used to serve administration requests to geoserver (via gui or REST)

The frontend specifies a small set of rules used to redirect the incoming requests to the right backend.
The rule::

  acl IS_ADM path_beg /geoserver_adm

Checks if the path begin with the /geoserver_adm string, if so the frontend will select the geoserver_adm backend via the::

  use_backend geoserver_adm if IS_ADM || IS_GEOADMIN

When the **geoserver_adm** backend is selected:

    #. A cookie called GEOADMIN is added to the request (here we have only 1 master instance so the value of this cookie will be ''1'')::
	
	cookie GEOADMIN insert

    #. The url is rewritten (ignoring case) as http://localhost:88/geoserver

	reqirep ^([^\ :]*)\ /geoserver_adm(.*)     \1\ /geoserver/\2

    #. Any further request will ship the cookie called GEOADMIN so next call to the http://localhost:88/geoserver URL will be redirected to the right backend (geoserver_adm).
	This is checked by the following rule::
	
	  acl IS_GEOADMIN hdr_sub(cookie) GEOADMIN

Then we have on the port 8088 the usual statistics monitor proxied by the listener called **admin** which can be accessed using the url:
    
    http://localhost:8088/haproxy?stats


    
The configuration is shown below::

    global
	log 127.0.0.1 local0
	log 127.0.0.1 local0 notice
	maxconn 2000
	user haproxy
	group haproxy
	daemon

    frontend http-in
	bind *:88
	mode http
	timeout client          1m
	# acl routing to backend
	  # check for the ADM backend
	  acl IS_GEOADMIN hdr_sub(cookie) GEOADMIN
	  acl IS_ADM path_beg /geoserver_adm

	# which backend
	  # ADM backend
	  use_backend geoserver_adm if IS_ADM || IS_GEOADMIN
	  # default backend
	  default_backend geoserver

    backend geoserver
	mode http
	balance roundrobin
	timeout connect         10s
	timeout server          1m
	server geoserver_1 localhost:8083/geoserver maxconn 1000 check port 8083
	server geoserver_2 localhost:8082/geoserver maxconn 1000 check port 8082

    backend geoserver_adm
	mode http
	option httpclose
	option httplog
	timeout connect         10s
	timeout server          1m
	balance roundrobin
	cookie GEOADMIN insert
	reqirep ^([^\ :]*)\ /geoserver_adm(.*)     \1\ /geoserver/\2
	server geoserver_1 localhost:8083/geoserver cookie 1 maxconn 30 check port 8083

    listen admin
	bind *:8088
	mode http
	stats enable
	timeout connect         10s
	timeout client          1m
	timeout server          1m
	stats uri /haproxy?stats

    defaults
	log     global
	mode http
	option httpclose
	option httplog
	option  dontlognull
	retries 3
	option redispatch
	timeout connect  5000
	timeout client  10000
	timeout server  10000

Options
^^^^^^^

The **option httpclose** enable or disable passive HTTP connection closing.

The **option httplog** enable logging of HTTP request, session state and timers.
 
The **log** directive mentions a syslog server to which log messages will be sent. On Ubuntu rsyslog is already installed and running but it doesn't listen on any IP address. We'll modify the config files of rsyslog later.

The **maxconn** directive specifies the number of concurrent connections on the frontend. The default value is 2000 and should be tuned according to your desired configuration.

The **user** and **group** directives changes the HAProxy process to the specified user/group.

The timeout directives
^^^^^^^^^^^^^^^^^^^^^^

The **connect** option specifies the maximum time to wait for a connection attempt to a VPS to succeed.

The **client** and **server** timeouts apply when the client or server is expected to acknowledge or send data during the TCP process.
HAProxy recommends setting the client and server timeouts to the same value.

The **retries** directive sets the number of retries to perform on a VPS after a connection failure.

The **stats** directives enable the connection statistics page and protects it with HTTP Basic authentication using the credentials specified by the stats auth directive.
This page can viewed with the URL mentioned in stats uri so in this case, it is http://localhost:8088/haproxy?stats

Configuring the logs
^^^^^^^^^^^^^^^^^^^^

In the global section we added a line::

  log 127.0.0.1 local0 notice

which sends syslog messages to the localhost IP address. But by default, rsyslog on Ubuntu doesn't listen on any address.
So we have to make it do so.

Edit the config file of rsyslog::

  sudo nano /etc/rsyslog.conf

Add/Edit/Uncomment the following lines::

  $ModLoad imudp
  $UDPServerAddress 127.0.0.1
  $UDPServerRun 514

Now rsyslog will work on UDP port 514 on address 127.0.0.1 but all HAProxy messages will go to /var/log/syslog so we have to separate them.

Create a rule for HAProxy logs::

  sudo nano /etc/rsyslog.d/haproxy.conf

Add the following line to it::

  if ($programname == 'haproxy') then -/var/log/haproxy.log

Now restart the rsyslog service::

  sudo service rsyslog restart

This writes all HAProxy messages and access logs to /var/log/haproxy.log

For any other details on options and configuration check the official documentation `here <http://haproxy.1wt.eu/download/1.4/doc/configuration.txt>`_