.. module:: geoserver.ogcapi.features.transactions
   :synopsis: Transactions specification future directions.

.. _geoserver.ogcapi.features.transactions:

Transactions on Features
==========================

Currently GeoServer provides no implementation for OGCAPI - Features Transactions specification. However, it might be of interest to have a notion about the direction which the standard is taking.


Two classes of transactions are being specified:

* **Create, Replace, Update and Delete**: this operations allows to perform changes on individual items in a collection.

* Atomic and Batch Transactions: complex transactions with atomic or batch semantics operating on, potentially, multiple resources across multiple collections.



Create, Replace, Update and Delete
-----------------------------------

.. list-table::
   :widths: 60,30,30


   * - **Endpoint**
     - **HTTPMethod**
     - **Operation**

   * - /collections/{collectionId}/items
     - POST
     - create a new item/Feature
   * - /collections/{collectionId}/items/{itemId}
     - PUT
     - replace an item/Feature
   * - /collections/{collectionId}/items/{itemId}
     - PATCH
     - partial update of an item/Feature
   * - /collections/{collectionId}/items/{itemId}
     - DELETE
     - delete an item/Feature


Examples of JSON DTO, for POST, PUT and PATCH operations

POST ``http://localhost:7070/geoserver/ogc/features/collections/sf:bugsites/items``

.. code-block:: json

   {
   "type":"Feature",
   "geometry":{
      "type":"Point",
      "coordinates":[
         -103.86761148,
         44.38484141
      ]
   },
   "geometry_name":"the_geom",
   "properties":{
      "cat":1,
      "str1":"Beetle site"
   }
 }


PUT ``http://localhost:7070/geoserver/ogc/features/collections/sf:bugsites/items/bugsites.1``

.. code-block:: json

   {
   "type":"Feature",
   "id":"bugsites.1",
   "geometry":{
      "type":"Point",
      "coordinates":[
         -103.86761148,
         44.38484141
      ]
   },
   "geometry_name":"the_geom",
   "properties":{
      "cat":1,
      "str1":"Beetle site"
   }
 }


PATCH ``http://localhost:7070/geoserver/ogc/features/collections/sf:bugsites/items/bugsites.1``

.. code-block:: json

  {
   "properties":{
      "cat":30,
      "str1":"Not a Beetle site"
   }
 }


Atomic and Batch transactions
-----------------------------

No specification has been provided yet. The services will provide the possibility to perform insertions, updates, replacements and deletes on multiple items at once.
The future specification will likely provide a dedicated endpoint, where to  POST a document describing the changes to operate on the collection (i.e. inserts, updates, replaces and/or deletes). Two modes will be available:

* ATOMIC, transactions should entirely succeeds or rolled back entirely if a single change fails.

* BATCH, each change operation will fail or succeed individually.
