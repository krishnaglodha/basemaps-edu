.. _geoserver.vector_data.filter:

Filtrage et extraction de données vectorielles
----------------------------------------------

WFS définit également des mécanismes pour récupérer uniquement un sous-ensemble des données qui correspondent à certaines contraintes spécifiées.

Mais avant d'aller dans les détails, nous allons examiner une autre façon de demander les informations du service au `` POST `` méthode HTTP. Pour cette question, nous allons utiliser l'extension Firefox appelé `Poster <https://addons.mozilla.org/en-US/firefox/addon/poster/>`_.

#. ouvrez Poster en cliquant sur le bouton "P" dans la barre de module:

   .. figure:: img/poster.png

      Ouvrez Poster

   .. figure:: img/poster-window.png
   
      La fenêtre Poster 

#. Définissez l'URL de ``http://localhost:8083/geoserver/wfs`` et aussi le **Content to Send**::
 
    <wfs:GetFeature xmlns:wfs='http://www.opengis.net/wfs'
      xmlns:ogc='http://www.opengis.net/ogc' service='WFS' version='1.0.0'>
      <Query typeName='geosolutions:WorldCountries'>
        <ogc:Filter>
          <ogc:FeatureId fid='WorldCountries.137' />
        </ogc:Filter>
      </Query>
    </wfs:GetFeature>

   .. figure:: img/poster-request.png

      Requête en Poster


#. Appuyez "POST", qui nous donne cette sortie

   .. figure:: img/poster-filter.png

      détails des états de Monaco en GML


#. Maintenant, nous allons écrire une demande équivalente - en utilisant le nom de l'état à la place de l' ``id``- en émettant ``GET`` et en codent le filtre dans un langage appelé `CQL <http://en.wikipedia.org/wiki/CQL>`_. Copiez l'adresse URL suivante dans la barre de navigation de votre navigateur ::
    http://localhost:8083/geoserver/wfs?request=GetFeature&service=WFS&version=1.0.0&typeName=geosolutions:WorldCountries&outputFormat=GML2&CQL_FILTER=NAME=%27Monaco%27

   .. figure:: img/cql-filter-url.png

      Le filtre CQL dans la barre d'adresse de Firefox

   .. figure:: img/cql_filter_result.png
 
      Les résultats du filtre CQL

Voilà comment un ensemble de fonctionnalités est filtrée avec soit l'encodage OGC ou la notation CQL 

Dans la section :ref:`next <geoserver.vector_data.wfst>` nous allons voir comment modifier les caractéristiques via un protocole appelé WFS transactionnel (WFS-T).

