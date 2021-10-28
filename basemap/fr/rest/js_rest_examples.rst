.. module:: geoserver.js_rest_examples
   :synopsis: Learn how to use the GeoServer REST module.

JavaScript REST examples
========================

The GeoServer REST API allows to perform CRUD operations against the GeoServer catalog via HTTP calls. It is possible to manage the resources representation using different formats, plain HTML (very limited and not useful to perform real CRUD operations), XML and JSON.
The latters in particular are very flexible and useful especially when writing GUIs allowing users to perform quick, but still complex, remote operations on GeoServer catalog.

JSON and HTTP protocol via AJAX in particular are very interesting when trying to build up HTML pages using simple JavaScript to manage GeoServer resources via REST.

In this section we will see a nice example of JSON parsing/mapping of the GeoServer resources for reding their attributes, other than AJAX functions used to perform read/write operations allowing a user to update layers defaultStyle with a simple click on the GUI.

JSON/AJAX REST example in JavaScript
------------------------------------

#. Open up a new browser window and go to the URL:

   .. code-block:: xml
      
      http://localhost:8083/geoserver/www/rest-example.html

   You should be able to see a very simple HTML page like the one depicted here below:
   
   .. figure:: img/rest_js1.png
     :width: 600

     Simple HTML page using JavaScript, JSON and AJAX to retrieve GeoServer styles and layers.

   As you may notice, if everything goes well, at startup there are two :guilabel:`select boxes` which are **dynamically** populated with the list of local GeoServer available styles and layers.

#. Select the style :guilabel:`line` from the *Available Styles* select box and :guilabel:`bstreets` from the *Available Layers* select box

   .. figure:: img/rest_js2.png
     :width: 600

   You should be able the selected names appear on the two :guilabel:`text boxes` in the bottom near the :guilabel:`Assign SLD ---> Layer` button.
   
#. Click on the button ::

         Assign SLD ---> Layer

   and confirm all the questions. You should get a *Done!* message at the end of the process.
   
   .. figure:: img/rest_js3.png
     :width: 600

#. Go to the `Map Preview <http://localhost:8083/geoserver/web/?wicket:bookmarkablePage=:org.geoserver.web.demo.MapPreviewPage>`_ page and select the :guilabel:`bstreets` OpenLayers preview

   .. figure:: img/rest_js4.png
     :width: 600

   notice that the default style of *Boulder Streets* layer is changed to plain blue lines.
   
   .. figure:: img/rest_js5.png
     :width: 300

#. No go back to the `rest js example <http://localhost:8083/geoserver/www/rest-example.html>`_ and repeat the operations above to set the :guilabel:`bstreets` style to :guilabel:`streets`

   .. figure:: img/rest_js6.png
     :width: 600

   and confirm from the `Map Preview <http://localhost:8083/geoserver/web/?wicket:bookmarkablePage=:org.geoserver.web.demo.MapPreviewPage>`_ that the default style has been changed back to the original one.
   
   .. note:: With the *streets* style you need to zoom in in order to see the boulder streets comeing up due to the scale denominator.

   .. figure:: img/rest_js7.png
     :width: 300

Taking a look at the concepts and code
--------------------------------------
First of all notice that the example has been made by using two files

.. code-block:: xml

   $GEOSERVER_DATA_DIR/www/rest-example.html
   $GEOSERVER_DATA_DIR/www/rest-example.js

The **/www** directory under the *$GEOSERVER_DATA_DIR* is a *special* directory. The GeoServer filters allow to stream out directly the content of this directory as if it was published on a web server.

We placed our files here since we used direct AJAX calls to GeoServer REST API, and as you may already know, in order to do this usually you **must** be in the same context due to the `Same-Origin policy <http://en.wikipedia.org/wiki/Same_origin_policy>`_ of the browsers.
Of course there are ways to allow AJAX calls from other origins/domains but is not of specific interest for this topic.

#. Open with a simple text editor the file *rest-example.html*. You will find a very simple HTML below

   .. code-block:: html
   
		<html xmlns="http://www.w3.org/1999/xhtml">
		  <head>
			<title>GeoServer Workshop</title>

			<script src="http://www.webtoolkit.info/djs/webtoolkit.base64.js"></script>
			<script src="rest-example.js"></script>

			<script language="Javascript">
			   var geoserverUsername='admin';
			   var geoserverPassword='***********';
			</script>
		  </head>

		  <body onload="init()">
			<h1 id="title">GeoServer Workshop - REST API trough JavaScript</h1>
			
			<h2>Available Styles</h2>
			   <select id="sldSelect" onchange="updateSelectedSld(this)"></select>
			
			<h2>Available Layers: 'geosolutions' workspace</h2>
			   <select id="lyrSelect" onchange="updateSelectedLyr(this)"></select>
			
			<br/><br/>
			SLD: <input type="text" id="selectedSld" style="witdh: 300px" readonly> -
			Layer: <input type="text" id="selectedLyr" style="witdh: 300px" readonly>
			<button id="assignSld" onclick="assignSldToLyr()">Assign SLD ----> Layer</button>
		  </body>

		</html>

   Take a look first to the **head** section. We load two JavaScript sources and define two global variables *geoserverUsername* and *geoserverPassword* which must be valued with the correct credentials of a user having access to GeoServer REST.

   .. Attention:: In common practice **never** place plain username and password on an HTML page. This is only for the purposes of this very simple exercise. You can use a simple form in order to provide credentials instead. Still be sure to pass the credentianls through a secure channel.
   
   The first file ::
   
        http://www.webtoolkit.info/djs/webtoolkit.base64.js

   is used to encode Base64 user credentials on the request headers for the Basic Auth.
   
   The second one ::
   
        rest-example.js

   is local to GeoServer context and contains our specific utility Javascript fucntions.
   
   The rest of the HTML is quite simple, just defines several simple HTML fields and invockes an ::
   
        init()

   function at startup.
   
#. Open with a simple text editor the file *rest-example.js*. Look for the *init()* function, which should be the first one

   .. code-block:: javascript
   
		/**
		 * init:function
		 * - main initialization function
		 **/
		function init(){
		 // Initializing the SLD list
		 var sldgetrequest=new ajaxRequest();
		 sldgetrequest.onreadystatechange=function(){
		 if (sldgetrequest.readyState==4){
			if (sldgetrequest.status==200 || window.location.href.indexOf("http")==-1){
			  var jsondata=eval("("+sldgetrequest.responseText+")") //retrieve result as an JavaScript object
			  var rssentries=jsondata.styles.style;

			  var sldSelect = document.getElementById('sldSelect');
				  sldSelect.options.length = 0; // clear out existing items
			  for (var i=0; i<rssentries.length; i++){
				var entry = rssentries[i];
				sldSelect.options.add(new Option(entry.name, i))
			  }  
			  document.getElementById('selectedSld').value=sldSelect.options[0].text;
			}
			else{
			  alert("An error has occured making the request");
			}
		   }
		  }

		  sldgetrequest.open("GET", "/geoserver/rest/styles.json", true);
		  sldgetrequest.setRequestHeader('Authorization', make_base_auth(geoserverUsername, geoserverPassword));
		  sldgetrequest.send(null);

		 // Initializing the Layers list
		 var lyrgetrequest=new ajaxRequest();
		 lyrgetrequest.onreadystatechange=function(){
		 if (lyrgetrequest.readyState==4){
			if (lyrgetrequest.status==200 || window.location.href.indexOf("http")==-1){
			  var jsondata=eval("("+lyrgetrequest.responseText+")") //retrieve result as an JavaScript object
			  var rssentries=jsondata.layers.layer;

			  var lyrSelect = document.getElementById('lyrSelect');
				  lyrSelect.options.length = 0; // clear out existing items
			  for (var i=0; i<rssentries.length; i++){
				var entry = rssentries[i];
				lyrSelect.options.add(new Option(entry.name, i))
			  }
			  document.getElementById('selectedLyr').value=lyrSelect.options[0].text;
			}
			else{
			  alert("An error has occured making the request");
			}
		   }
		  }

		  lyrgetrequest.open("GET", "/geoserver/rest/layers.json", true);
		  lyrgetrequest.setRequestHeader('Authorization', make_base_auth(geoserverUsername, geoserverPassword));
		  lyrgetrequest.send(null);
		}   

   The function is quite simple, makes two REST GET operations to read styles and layers from GeoServer and populate the select boxes.
   
   Notice how the JSON responses from GeoServer are parsed and converted to JavaScript objects using the ::
   
          eval()

   native Javascript function.
   
   Lets examine the styles JSON response from GeoServer REST API; if you go to this URL on a standard browser ::
   
          http://localhost:8083/geoserver/rest/styles.json

   you will get a plain string like this
   
   .. code-block:: json

		{
		  "styles":{
			"style":[
			  {
				"name":"arealandmarks",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/arealandmarks.json"
			  },
			  {
				"name":"arealandmarks_pt",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/arealandmarks_pt.json"
			  },
			  {
				"name":"buildings",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/buildings.json"
			  },
			  {
				"name":"cemetery_graphics",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/cemetery_graphics.json"
			  },
			  {
				"name":"cemetery_mark",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/cemetery_mark.json"
			  },
			  {
				"name":"citylimits",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/citylimits.json"
			  },
			  {
				"name":"contours",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/contours.json"
			  },
			  {
				"name":"countries",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/countries.json"
			  },
			  {
				"name":"county",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/county.json"
			  },
			  {
				"name":"dem",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/dem.json"
			  },
			  {
				"name":"dem2",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/dem2.json"
			  },
			  {
				"name":"dem_elevation",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/dem_elevation.json"
			  },
			  {
				"name":"hillshade",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/hillshade.json"
			  },
			  {
				"name":"lakes",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/lakes.json"
			  },
			  {
				"name":"line",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/line.json"
			  },
			  {
				"name":"mainrd",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/mainrd.json"
			  },
			  {
				"name":"parcels",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/parcels.json"
			  },
			  {
				"name":"point",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/point.json"
			  },
			  {
				"name":"point_landmark",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/point_landmark.json"
			  },
			  {
				"name":"point_landmark_ds",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/point_landmark_ds.json"
			  },
			  {
				"name":"point_landmark_ds_ns",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/point_landmark_ds_ns.json"
			  },
			  {
				"name":"polygon",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/polygon.json"
			  },
			  {
				"name":"raster",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/raster.json"
			  },
			  {
				"name":"rivers",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/rivers.json"
			  },
			  {
				"name":"river_arrow",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/river_arrow.json"
			  },
			  {
				"name":"states_population",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/states_population.json"
			  },
			  {
				"name":"streets",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/streets.json"
			  },
			  {
				"name":"streets_inner",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/streets_inner.json"
			  },
			  {
				"name":"streets_outer",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/streets_outer.json"
			  },
			  {
				"name":"trails",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/trails.json"
			  },
			  {
				"name":"trails2",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/trails2.json"
			  },
			  {
				"name":"wetlands",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/wetlands.json"
			  },
			  {
				"name":"wetlands_dyn",
				"href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/wetlands_dyn.json"
			  }
			]
		  }
		}

   which is converted by *eval()* to a complex Javascript object, that can be used like a normal variable to inspect and also update it's properties. ::
           
           jsondata.styles.style

   can be used to get the styles array. Each entry has a property *name* which is used later to populate the select box *options*
   
   .. code-block:: javascript

			  for (var i=0; i<rssentries.length; i++){
				var entry = rssentries[i];
				lyrSelect.options.add(new Option(entry.name, i))
			  }

   The same works for the layers ::
   
          http://localhost:8083/geoserver/rest/layers.json

   .. note:: Notice that all the request have been issued by setting the authorization ::
   
                .setRequestHeader('Authorization', make_base_auth(geoserverUsername, geoserverPassword));

             in order to avoid GeoServer to ask for credentials. This is also necessary later for the PUT request, otherwise you will get an authorization exception.

#. Search now for the for the *assignSldToLyr()* function inside the *rest-example.js*

   .. code-block:: javascript
   
		/**
		 * assignSldToLyr:function
		 * - retrieves the layer JSON and updates it via REST PUT by modifying the defaultStyle
		 */
		function assignSldToLyr(){
		  var sld = document.getElementById('selectedSld').value;
		  var lyr = document.getElementById('selectedLyr').value;
		  if (confirm('Assign SLD: "' + sld + '" to Layer: "' + lyr + '" ?')){
			// retrieving Layer JSON
			var layer;
			var layerAjaxRequest=new ajaxRequest();
			layerAjaxRequest.onreadystatechange=function(){
			  if (layerAjaxRequest.readyState==4){
				if (layerAjaxRequest.status==200 || window.location.href.indexOf("http")==-1){
				  var jsondata=eval("("+layerAjaxRequest.responseText+")") //retrieve result as an JavaScript object
				  layer=jsondata.layer;
				}
				else{
				  alert("An error has occured making the request");
				}
			  }
			}
			layerAjaxRequest.open("GET", "/geoserver/rest/layers/"+lyr+".json", false); //synchronous request
			layerAjaxRequest.setRequestHeader('Authorization', make_base_auth(geoserverUsername, geoserverPassword));
			layerAjaxRequest.send(null);

			if(confirm('I\'m going to change the style of the layer "' + lyr + '" from "' + layer.defaultStyle.name + '" to "' + sld + '". Would you like to proceed?')){
			   // performing PUT (update) request to change the Layer default style
			   var mypostrequest=new ajaxRequest();
			   mypostrequest.onreadystatechange=function(){
				 if (mypostrequest.readyState==4){
				   if (mypostrequest.status==200 || window.location.href.indexOf("http")==-1){
					 alert("Done!");
				   }
				   else{
					 alert("An error has occured making the request");
				   }
				 }
			   }

			   // updating layer defaultStyle
			   layer.defaultStyle.name=sld;
			   
			   // sending out the request
			   mypostrequest.open("PUT", "/geoserver/rest/layers/"+lyr+".json", true);
			   mypostrequest.setRequestHeader("Content-type", "application/json");
			   mypostrequest.setRequestHeader('Authorization', make_base_auth(geoserverUsername, geoserverPassword));
			   mypostrequest.send('{"layer":' + JSON.stringify(layer) + '}');
			}
		  }
		}

   This function is quite interesting. It executes two REST operation, a GET in order to retrieve the selected layer details in JSON, and a PUT in order to update the *defaultStyle* by modifying the JSON layer representation and sending back the updated string to GeoServer.
   
   The JSON representation of a layer can be obtained simply by issuing a HTTP GET request like this ::
   
             http://localhost:8083/geoserver/rest/layers/bstreets.json

   you will get back something like
   
   .. code-block:: json
   
		{
		  "layer":{
			"name":"bstreets",
			"type":"VECTOR",
			"defaultStyle":{
			  "name":"streets",
			  "href":"http:\/\/localhost:8181\/geoserver\/rest\/styles\/streets.json"
			},
			"resource":{
			  "@class":"featureType",
			  "name":"bstreets",
			  "href":"http:\/\/localhost:8181\/geoserver\/rest\/workspaces\/geosolutions\/datastores\/boulder_shapefiles\/featuretypes\/bstreets.json"
			},
			"enabled":true,
			"queryable":true,
			"metadata":{
			  "entry":[
				{
				  "@key":"GWC.metaTilingX",
				  "$":"4"
				},
				{
				  "@key":"GWC.autoCacheStyles",
				  "$":"true"
				},
				{
				  "@key":"GWC.metaTilingY",
				  "$":"4"
				},
				{
				  "@key":"GWC.gutter",
				  "$":"0"
				},
				{
				  "@key":"advertised",
				  "$":"true"
				},
				{
				  "@key":"GWC.enabled",
				  "$":"true"
				},
				{
				  "@key":"GWC.gridSets",
				  "$":"EPSG:4326,EPSG:900913"
				},
				{
				  "@key":"GWC.cacheFormats",
				  "$":"image\/jpeg,image\/png"
				}
			  ]
			},
			"attribution":{
			  "logoWidth":0,
			  "logoHeight":0
			}
		  }
		}
   
   The function simply updates the layer defaultStyle with the one selected by the user ::
   
                // updating layer defaultStyle
                layer.defaultStyle.name=sld;

   and then issues an authorized HTTP PUT request back to GeoServer at the same json layer URL ::
   
                // sending out the request
                mypostrequest.open("PUT", "/geoserver/rest/layers/"+lyr+".json", true);
                mypostrequest.setRequestHeader("Content-type", "application/json");
                mypostrequest.setRequestHeader('Authorization', make_base_auth(geoserverUsername, geoserverPassword));
                mypostrequest.send('{"layer":' + JSON.stringify(layer) + '}');

   .. note:: 
   
      It's quite interesting the use of ::

             JSON.stringify(layer)
      
      in order to convert the JSON Object back to a plain string. We also need to insert the ::

             {"layer": <body here> }

      strings in order to match the original format, since we filtered out before the "*layer*" part by doing this ::

             layer=jsondata.layer;

      Notice also that the URL address for the update operation is the same we used previously to retrieve the layer Json representation ::

             /geoserver/rest/layers/"+lyr+".json

      which in the practice is equivalent to something like ::

             http://localhost:8083/geoserver/rest/layers/<selected_layer>.json

.