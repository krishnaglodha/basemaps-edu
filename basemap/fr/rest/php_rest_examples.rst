.. module:: geoserver.php_rest_examples
   :synopsis: Learn how to use the GeoServer REST module.


 
 
PHP REST examples
=================

PHP has cURL functions , as well as XML functions , making it a convenient method for performing batch processing through the Geoserver REST interface.
The following scripts execute single requests, but can be easily modified with looping structures to perform batch processing.

.. note:: For this workshop the needed packages are already installed and configured. To follow this exemple on other systems you may have to install php and php curl support executing the following code::

		  $sudo apt-get install php5 php5-curl php5-cli
		
		

POST with PHP/cURL
------------------

To add a new workspace to geoserver using REST interface you need to

#. Create a new file named :file:`php_post_example.php` filled with the code below 
	.. code-block:: php
	
		#!/usr/bin/php
		<?php
			// Open log file
			$logfh = fopen("GeoserverPHP.log", 'w') or die("can't open log file");

			// Initiate cURL session
			$service = "http://localhost:8083/geoserver/"; 
			$request = "rest/workspaces"; // to add a new workspace
			$url = $service . $request;
			$ch = curl_init($url);

			// Optional settings for debugging
			curl_setopt($ch, CURLOPT_RETURNTRANSFER, true); //option to return string
			curl_setopt($ch, CURLOPT_VERBOSE, true);
			curl_setopt($ch, CURLOPT_STDERR, $logfh); // logs curl messages

			//Required POST request settings
			curl_setopt($ch, CURLOPT_POST, True);
			$passwordStr = "geosolutions:Geos"; 
			curl_setopt($ch, CURLOPT_USERPWD, $passwordStr);

			//POST data
			curl_setopt($ch, CURLOPT_HTTPHEADER,
					  array("Content-type: application/xml"));
			$xmlStr = "<workspace><name>test_php</name></workspace>";
			curl_setopt($ch, CURLOPT_POSTFIELDS, $xmlStr);

			//POST return code
			$successCode = 201;

			$buffer = curl_exec($ch); // Execute the curl request

			// Check for errors and process results
			$info = curl_getinfo($ch);
			if ($info['http_code'] != $successCode) {
			  $msgStr = "# Unsuccessful cURL request to ";
			  $msgStr .= $url." [". $info['http_code']. "]\n";
			  fwrite($logfh, $msgStr);
			} else {
			  $msgStr = "# Successful cURL request to ".$url."\n";
			  fwrite($logfh, $msgStr);
			}
			fwrite($logfh, $buffer."\n");

			curl_close($ch); // free resources if curl handle will not be reused
			fclose($logfh);  // close logfile

		?>

#. To execute the code above open the terminal and execute::

	$php php_post_example.php
	
#. You can see a new file in the current directory named `GeoserverPHP.log` with this content:
	.. code-block:: text
	
		* About to connect() to localhost port 8083 (#0)
		*   Trying 127.0.0.1... * connected
		* Server auth using Basic with user 'admin'
		> POST /geoserver/rest/workspaces HTTP/1.1
		Authorization: Basic YWRtaW46Z2Vvc2VydmVy
		Host: localhost:8083
		Accept: */*
		Content-type: application/xml
		Content-Length: 44

		* upload completely sent off: 44out of 44 bytes
		< HTTP/1.1 201 Created
		< Date: Thu, 24 May 2012 13:45:05 GMT
		< Location: http://localhost:8083/geoserver/rest/workspaces/test_php
		< Server: Noelios-Restlet-Engine/1.0..8
		< Transfer-Encoding: chunked
		< 
		* Connection #0 to host localhost left intact
		# Successful cURL request to http://localhost:8083/geoserver/rest/workspaces

		* Closing connection #0

GET with PHP/cURL
------------------
The script above can be modified to perform a GET request to obtain the names of all workspaces by replacing the code blocks for required settings, data and return code.
So you can create another file named :file:`php_get_example.php` with the following content:

	.. code-block:: php
	
		#!/usr/bin/php
		<?php
			// Open log file
			$logfh = fopen("GeoserverPHP.log", 'w') or die("can't open log file");

			// Initiate cURL session
			$service = "http://localhost:8083/geoserver/"; 
			$request = "rest/workspaces"; 
			$url = $service . $request;
			$ch = curl_init($url);

			// Optional settings for debugging
			curl_setopt($ch, CURLOPT_RETURNTRANSFER, true); 
			curl_setopt($ch, CURLOPT_VERBOSE, true);
			curl_setopt($ch, CURLOPT_STDERR, $logfh); 

			//Required GET request settings
			$passwordStr = "geosolutions:Geos"; 
			curl_setopt($ch, CURLOPT_USERPWD, $passwordStr);

			 //GET data
			curl_setopt($ch, CURLOPT_HTTPHEADER, array("Accept: application/xml"));

			//GET return code
			$successCode = 200;

			$buffer = curl_exec($ch); 

			// Check for errors and process results
			$info = curl_getinfo($ch);
			if ($info['http_code'] != $successCode) {
			  $msgStr = "# Unsuccessful cURL request to ";
			  $msgStr .= $url." [". $info['http_code']. "]\n";
			  fwrite($logfh, $msgStr);
			} else {
			  $msgStr = "# Successful cURL request to ".$url."\n";
			  fwrite($logfh, $msgStr);
			}
			fwrite($logfh, $buffer."\n");

			curl_close($ch); 
			fclose($logfh);  

		?>


#. execute this script::

	$ php php_get_example.php
		
			
#. this time the logfile should look something like:

	.. code-block:: text
	
		* About to connect() to localhost port 8083 (#0)
		*   Trying 127.0.0.1... * connected
		* Server auth using Basic with user 'admin'
		> GET /geoserver/rest/workspaces HTTP/1.1
		Authorization: Basic YWRtaW46Z2Vvc2VydmVy
		Host: localhost:8083
		Accept: application/xml

		< HTTP/1.1 200 OK
		< Date: Thu, 24 May 2012 14:23:58 GMT
		< Server: Noelios-Restlet-Engine/1.0..8
		< Content-Type: application/xml
		< Transfer-Encoding: chunked

		< 

		* Connection #0 to host localhost left intact
		# Successful cURL request to http://localhost:8083/geoserver/rest/workspaces
		<workspaces>
		  <workspace>
			<name>test_ws</name>
			<atom:link xmlns:atom="http://www.w3.org/2005/Atom" rel="alternate" href="http://localhost:8083/geoserver/rest/workspaces/test_ws.xml" type="application/xml"/>
		  </workspace>
		  <workspace>
			<name>test_php</name>
			<atom:link xmlns:atom="http://www.w3.org/2005/Atom" rel="alternate" href="http://localhost:8083/geoserver/rest/workspaces/test_php.xml" type="application/xml"/>
		  </workspace>
		  <workspace>
			<name>geosolutions</name>
			<atom:link xmlns:atom="http://www.w3.org/2005/Atom" rel="alternate" href="http://localhost:8083/geoserver/rest/workspaces/geosolutions.xml" type="application/xml"/>
		  </workspace>
		</workspaces>
		* Closing connection #0
	




					
