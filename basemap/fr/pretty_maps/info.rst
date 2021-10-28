.. _geoserver.info:


Avoir Accès aux Informations des Cartes
---------------------------------------

Cette séction décrit comment utiliser le système de modèles de GeoServer pour créer des réponses HTML personalisées GetFeatureInfo. GetFeatureInfo est un appel WMS standard qui vous permet de récupérer les informations sur les traits et les couvertures montrées dans une carte.

La carte peut etre composée par plusieurs couches, et GetFeatureInfo peut etre enseigné à retourner plusieurs descriptions de fonction, qui peuvent etre de deux types différents. GetFeatureInfo peut générer des sorties dans des formats différents: GML2, texte simple et HTML.

Donner un modèle est impliqué dans celui HTML.


#. Allez jusq'à **Layer preview** pour montrer la couche :guilabel:`geosolutions:bplandmarks`.

#. cliquez pour example sur le :guilabel:`Rocky Mountain Natl Park` région dans la carte OpenLayers pour montrer les FeatureInfo.


   .. figure:: img/info1.png
         
      Par défaut demandez GetFeatureInfo 

#. Afin de configurer un modèle personalisé des résultats de GetFeatureInfo créez trois fichiers. FTL dans :guilabel:`$GEOSERVER_DATA_DIR/workspaces/geosolutions` répertoire nommé::

	- header.ftl
  	- content.ftl
	- footer.ftl


   .. note:: The Modèle est géré à l'aide de `Freemarker <http://freemarker.sourceforge.net/>`_. Celui là est un symple mais puissant moteur de modèles que GeoServer utilise chaque fois que les développeurs autorisent une personnalisation par l'utilisateur de sorties textuelles. En particulier, pendant l'écriture on peut pérsonaliser les sorties de GetFeatureInfo, GeoRSS et KML.
   
   .. note:: Le fractionnement du modèle en trois fichiers permet à l'administrateur de tenir un style cohérent pour les résultats de GetFeatureInfo, mais d'utiliser des modèles différents pour des différents espaces de travail ou des différents couches: cela est obtenu en fournissant un fichier maître ``header.ftl`` et ``footer.ftl``, mais en spécifiant ``content.ftl`` pour chaque couche.

#. Dans le fichier :guilabel:`header.ftl` introduisez le HTML suivant:

   .. code-block:: html

	<#--
	Section d'en-tête de la sortie GetFeatureInfo HTML. On devrait avoir la séction <head>, et un début
	de <body>. Dans le cas que le css utilise une classe spéciale pour featureInfo,
	dès que le HTML généré peut se mélanger avec une autre page qui pourrait changer son aspect si on utilise des classes génériques
	comme td, tr, etc.
	-->
	<html>
  		<head>
    			<title>Geoserver GetFeatureInfo output</title>
  		</head>
  		<style type="text/css">
        		table.featureInfo, table.featureInfo td, table.featureInfo th {
                		border:1px solid #ddd;
                		border-collapse:collapse;
                		margin:0;
                		padding:0;
                		font-size: 90%;
                		padding:.2em .1em;
        		}
        
			table.featureInfo th{
            			padding:.2em .2em;
                		text-transform:uppercase;
                		font-weight:bold;
                		background:#eee;
        		}
        
			table.featureInfo td{
                		background:#fff;
        		}
	
        		table.featureInfo tr.odd td{
                		background:#eee;
        		}
	
       			table.featureInfo caption{
                		text-align:left;
                		font-size:100%;
                		font-weight:bold;
                		text-transform:uppercase;
                		padding:.2em .2em;
        		}
  		</style>
  		<body>

#. Dans le fichier :guilabel:`content.ftl` introduisez le suivant HMTL:

   .. code-block:: html

	<ul>
	<#list features as feature>
  		<li><b>Type: ${type.name}</b> (id: <em>${feature.fid}</em>):
  		<ul>
  		<#list feature.attributes as attribute>
    			<#if !attribute.isGeometry>
      				<li>${attribute.name}: ${attribute.value}</li>
    			</#if>
  		</#list>
  		</ul>
  		</li>
	</#list>
	</ul>

#. Dans le fichier :guilabel:`footer.ftl` introduisez le suivant HMTL:

   .. code-block:: html

	<#--
	Titre en bas de page de la séction de la sortie GetFeatureInfo HTML. ça devrait fermer le corps et l'étiquette html.
	-->
  		</body>
	</html>


#. Allez à l'avant-première de la carte pour montrer la couche :guilabel:`geosolutions:bplandmarks`.

#. cliquez sur :guilabel:`Rocky Mountain Natl Park` région dans les cartes OpenLayers pour montrer la nouvelle representation des FeatureInfo.

   .. figure:: img/info2.png
         
      Personaliser le modèle GetFeatureInfo
