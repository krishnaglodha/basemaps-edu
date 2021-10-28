.. module:: inspire.status

.. _inspire.status:


Status of INSPIRE Compliance for GeoServer
------------------------------------------

GeoServer supports directly, with a different degree of compliance, a number of INSPIRE Services, moreover it can be used as the baseline to build some others. Before going into more details about what is supported
it is important to point out that GeoServer does not provide any support at this stage for Discovery Services, although it is planned for the next releases.

Download Services 
=================

GeoServer implements **OGC WFS 2.0** and **GML 3.2.1** specification since the ``2.2.x`` release series. Here are some additional facts about WFS 2.0 compliance in GeoServer:

* SOAP is supported
* Standard Capabilities Extensions is missing
* Locate and Remote Resolve are missing
* GetPropertyValue interactions with AppSchema/Complex Features is missing

View Services 
=============
GeoServer implements natively **OGC WMS 1.1.1**, **1.3** and **OGC WMTS 1.0.0** (through the integrated GeoWebCache) already on the stable ``2.1.x`` series. However, although a specific INSPIRE extension has also been created in order to provide better support for the INSPIRE requirements found in the View Service Technical Guideline, compliance is still partial:

* SOAP Support is missing (notice however, that it is not mandatory but recommended)
* Scenario 2 for View Services (direct metadata in capabilities documents) is not supported
* There is partial support for multilingualism (though the extension mentioned above)

  * Support for one single language
  * No support for multilingual metadata on layers
  * Missing localized support for exceptions
  * Missing localization support for contents (e.g., GetFeatureInfo, GetMap labels)
    
* Support for Symbology Encoding 1.1 is present but still partial
* Support for SLD 1.0 is very mature and robust

Transformation Service 
======================
GeoServer provides coordinate transformation tools with the **gs:Reproject WPS process**, however it would requires some changes to become INSPIRE compliant:

* Name change
* List supported SRS
* Use different mime types for GML
* Add *test transformation* mode (does not actually transform, checks only if possible)

Invoke Spatial Service Services
===============================
GeoServer Support **OGC WPS 1.0.0** which can be used as a base for the InvokeSD services. Specifically:

* Interaction with external WFS and WCS is supported
* Automatic Ingestion of produced data is supported
* Basic process chaining is supported
* Interaction with BPEL/BPMN engine is to be tested

