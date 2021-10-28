.. module:: clustering.active.advancedbrokerURI

.. _clustering.active.advancedbrokerURI:

Advanced Configuration for the Broker URIs
====================================================

In this section we are going to provide some advanced information on the the URIs that can be used to configure the link from a Slave Instance to the Broker.
	
TCP
----------

tcp://host:port

Client connects to the broker at the given URL

**Supported Options:**

  - trace (false) Causes all commands that are sent over the transport to be logged
  - daemon (false) Tells the transport thread to run as a daemon or not. Useful to enable when embedding in a Spring container or a web container to allow the container to shut down properly.
  - useLocalHost (false) When true, it causes the local machines name to resolve to "localhost".
  - socketBufferSize (64 * 1024) Sets the socket buffer size in bytes
  - keepAlive (false) When true, enables TCP KeepAlive on the broker connection. Useful to ensure that inactive consumers don't time out.
  - soTimeout (0) sets the socket timeout in milliseconds
  - connectionTimeout (30000) A non-zero value specifies the connection timeout in milliseconds. A zero value means wait forever for the connection to be established. Negative values are ignored.
  - wireFormat (default) The name of the WireFormat to use
  - wireFormat.* All the properties with this prefix are used to configure the wireFormat. See Configuring Wire Formats for more information
  - closeAsync (true) The false value causes all sockets to be closed synchronously
  - soLinger (MIN_INTEGER) When > -1 causes the socket soLinger option to be enabled with this value. When == -1, causes soLinger to be disabled. (from 5.6.0)
  - maximumConnections (Integer.MAX_VALUE) The maximum number of sockets the broker is allowed to create
  - tcpNoDelay (true) When true, the TCP_NODELAY setting is enabled on the socket.

**Example URI**

  - On Server side (in TransportConnector)::
  
tcp://localhost:61616?transport.trace=false&transport.soTimeout=60000

  - On Client side::
  
tcp://localhost:61616?trace=false&soTimeout=60000
    
	
Failover Transport
--------------------

failover:(Uri1,Uri2,Uri3,...,UriN)

Provides a list of possible URIs to connect to and one of them is randomly chosen. If the connection fails then the transport auto-reconnects to a different one. This is highly recommended in combination with the tcp protocol to work with an HA active-passive brokers configuration.

You can add options to the various URLs. Previously, if you wanted to detect dead connections faster you had to add the wireFormat.maxInactivityDuration=1000 option to all the nested URLs in the fail-over list. For example:

**Supported Options:**
  - updateClusterClients (false) if true, pass information to connected clients about changes in the topology of the broker cluster
  - rebalanceClusterClients (false) if true, connected clients will be asked to rebalance across a cluster of brokers when a new broker joins the network of brokers (note: priorityBackup=true can override)
  - updateClusterClientsOnRemove (false) if true, will update clients when a cluster is removed from the network. Having this as separate option enables clients to be updated when new brokers join, but not when brokers leave.
  - updateClusterFilter (null) comma separated list of regular expression filters used to match broker names of brokers to designate as being part of the fail-over cluster for the clients
  - initialReconnectDelay (10) How long to wait before the first reconnect attempt (in ms)
  - maxReconnectDelay (30000) The maximum amount of time we ever wait between reconnect attempts (in ms)
  - useExponentialBackOff (true) Should an exponential backoff be used between reconnect attempts
  - reconnectDelayExponent (2.0) The exponent used in the exponential backoff attempts
  - maxReconnectAttempts (-1 | 0) -1 is default and means retry forever, 0 means don't retry (only try connection once but no retry). If set to >0, then this is the maximum number of reconnect attempts before an error is sent back to the client.
  - startupMaxReconnectAttempts (0) If not 0, then this is the maximum number of reconnect attempts before an error is sent back to the client on the first attempt by the client to start a connection, once connected the maxReconnectAttempts option takes precedence.
  - randomize (true) use a random algorithm to choose the the URI to use for reconnect from the list provided
  - backup (false) initialize and hold a second transport connection - to enable fast fail-over
  - timeout (-1) Enables timeout on send operations (in milliseconds) without interruption of reconnection process
  - trackMessages (false) keep a cache of in-flight messages that will flushed to a broker on reconnect
  - maxCacheSize (131072) size in bytes for the cache, if trackMessages is enabled
  - updateURIsSupported (true) Determines whether the client should accept updates to its list of known URIs from the connected broker. Added in ActiveMQ 5.4
  - updateURIsURL (null) A URL (or path to a local file) to a text file containing a comma separated list of URIs to use for reconnect in the case of failure. Added in ActiveMQ 5.4
  - nested.* (null) Extra options to add to the nested URLs.
  - warnAfterReconnectAttempts.* (10) After every N reconnect attempts log a warning to indicate there is no connection but that we are still trying, set to <= 0 to disable. Added in ActiveMQ 5.10
  - reconnectSupported (true) Determines whether the client should respond to broker ConnectionControl events with a reconnect (see: rebalanceClusterClients)

**Example URI**::
  
failover:(tcp://localhost:61616,tcp://remotehost:61616)?initialReconnectDelay=100

  
Discovery 
-----------

discovery://

The Discovery transport works just like the Failover transport, except that it uses a discovery agent to locate the list of uri to connect to. The Discovery transport is also used by the Fanout transport for discovering brokers to send a fanout message to.

**Configuration Syntax**

discovery://(discoveryAgentURI)?transportOptions

Note that to be able to use Discovery to find brokers, the brokers need to have the multicast discovery agent enabled on the broker.

**Supported Options**
  - reconnectDelay (10) How long to wait for discovery
  - initialReconnectDelay (10) How long to wait before the first reconnect attempt to a discovered url
  - maxReconnectDelay (30000) The maximum amount of time we ever wait between reconnect attempts
  - useExponentialBackOff (true) Should an exponential backoff be used between reconnect attempts
  - backOffMultiplier	(2) The exponent used in the exponential backoff attempts
  - maxReconnectAttempts (0) If not 0, then this is the maximum number of reconnect attempts before an error is sent back to the client
  - group (default) an identifier for the group to partition multi cast traffic among collaborating peers; the group forms part of the shared identity of a discovery datagram      
    

    
Refer to `this page <http://activemq.apache.org/uri-protocols.html>`_ for a complete list of supported protocols.
  



