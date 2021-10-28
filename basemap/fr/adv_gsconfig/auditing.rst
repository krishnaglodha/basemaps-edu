.. geoserver.logging:

Inscrire toute les requetes sur le fichier système 
==================================================

Le mode histoire registre toutes les requetes dans une base de données. Cela peut mettre une très importante pression
sur la base de données et peut conduire à l'introduction problèmes d'insertion car la table de requête commente à accueillir
millions de dossiers.

Une alternative au mode histoire est constituée par permettre l'enregistreur d'audit, qui connecterà 
les details de chaque requete dans un fichier, qui est periodiquement roulé. Applications secondaires peuvent en suite processer
ces fichiers enregistrés et construire des résumés ad hoc hors ligne.

Configuration
-------------

Le fichier ``monitor.properties`` peut contenir les éléments suivants pour activer et configurer le fichier d'audit:

#. Allez à ${GEOSERVER_DATA_DIR}/monitoring et ouvrez le ``monitor.properties`` puis ajoutez la configuration suivante::

     audit.enabled=true
     audit.path=${TRAINING_ROOT}
     audit.roll_limit=20

#. Allez à `Map Preview <http://localhost:8083/geoserver/web/?wicket:bookmarkablePage=:org.geoserver.web.demo.MapPreviewPage>`_ and open the `geosolutions:states` layer clicking on the ``OpenLayer`` link.

#. Zoomez la carte plusieurs fois.

#. Ouvrez le nouveau fichier log (named like ``geoserver_audit_yyyymmdd_nn.log``) situé à ${TRAINING_ROOT}. 

   .. note::

      - **audit.enable**: est utilisé pour allumer l'enregistreur (il est désactivée par défaut).
      - **audit.path**: est le répertoire où les fichiers log seront créés.
      - **audit.roll_limit**: est le nombre de demandes enregistrées dans un fichier avant que le roulage se passe. 
     
   .. note:: Les fichiers sont automatiquement roulés aussi au au début de chaque journée.

Sorties et contenus
--------------------

Le répertoire enregistré contiendra un numéro de fichiers log après le modèle ``geoserver_audit_yyyymmdd_nn.log``.
Le ``nn`` est augmentée à chaque rouleau de l'image. Le contenu du  répertoire log ressemblerà à::

      geoserver_audit_20110811_2.log
      geoserver_audit_20110811_3.log
      geoserver_audit_20110811_4.log
      geoserver_audit_20110811_5.log
      geoserver_audit_20110811_6.log
      geoserver_audit_20110811_7.log
      geoserver_audit_20110811_8.log
	
Par défaut chaque contenu de fichier log serà un document xml ressemblant au texte suivant::
  
	<?xml version="1.0" encoding="UTF-8" ?>
	<Requests>
		<Request id="168">
		   <Service>WMS</Service> 
		   <Version>1.1.1</Version>
		   <Operation>GetMap</Operation> 
		   <SubOperation></SubOperation>
		   <Resources>GeoSolutions:elba-deparea</Resources>
		   <Path>/GeoSolutions/wms</Path>
		   <QueryString>LAYERS=GeoSolutions:elba-deparea&amp;STYLES=&amp;FORMAT=image/png&amp;TILED=true&amp;TILESORIGIN=9.916,42.312&amp;SERVICE=WMS&amp;VERSION=1.1.1&amp;REQUEST=GetMap&amp;EXCEPTIONS=application/vnd.ogc.se_inimage&amp;SRS=EPSG:4326&amp;BBOX=9.58375,42.64425,9.916,42.9765&amp;WIDTH=256&amp;HEIGHT=256</QueryString>
		   <HttpMethod>GET</HttpMethod>
		   <StartTime>2011-08-11T20:19:28.277Z</StartTime> 
		   <EndTime>2011-08-11T20:19:28.29Z</EndTime>
		   <TotalTime>13</TotalTime> 
		   <RemoteAddr>192.168.1.5</RemoteAddr>
		   <RemoteHost>192.168.1.5</RemoteHost>
		   <Host>demo1.geo-solutions.it</Host> 
		   <RemoteUser>admin</RemoteUser>
		   <ResponseStatus>200</ResponseStatus>
		   <ResponseLength>1670</ResponseLength>
		   <ResponseContentType>image/png</ResponseContentType>
		   <Failed>false</Failed>
		</Request>
		...
	</Requests>

Personnaliser les contenus du log
---------------------------------

Les contenus du log sont entraînés par trois modèles FreeMarker. Nous pouvons les personaliser pour avoir un fichier log qui soit par example un fichier csv.

#. Sur le système de fichiers naviguez au répertoire de données GeoServer situé en $GEOSERVER_DATA_DIR.

#. Dans le répertoire ``monitoring``apa créez un nouveau fichier nommé ``header.ftl`` (il est utilisé une seule fois quand on crée un nouveau fichier log pour former les premières lignes du fichier). 

#. Ouvrez ``header.ftl`` dans l'éditeur de texte de votre choix et introduisez le contenu suivant::

	# l'heure de début, les services,la version, l'operation, url,la reponse, le type de contenu, le temps total, la longueur de la reponse, l'indicateur d'erreur
	
#. Créez un autre fichier nommé ``content.ftl``.

#. Ouvrez ``content.ftl`` dans l'éditeur de texte de votre choix et introduisez le contenu suivant::

	${startTime?datetime?iso_utc_ms},${service!""},${owsVersion!""},${operation!""},"${path!""}${queryString!""}",${responseContentType!""},${totalTime},${responseLength?c},<#if error??>failed<#else>success</#if>
	</#escape>
    
#. Créez un dernier fichier nommé ``footer.ftl``, et laissez-le vide 

#. Exécutez de nouveau quelques requetes, le fichier log devrait contenir quelque chose comme celle qui suit::

    # start time,services,version,operation,url,response content type,total time,response lenght,error flag
    2012-06-07T10:37:09.725Z,WMS,1.1.1,GetMap,"/geosolutions/wmsLAYERS=geosolutions:ccounties&STYLES=&FORMAT=image/png&SERVICE=WMS&VERSION=1.1.1&REQUEST=GetMap&SRS=EPSG:4269&BBOX=-106.17254516602,39.489453002927,-105.18378466798,40.054948608395&WIDTH=577&HEIGHT=330",image/png,59,30420,success
    2012-06-07T10:37:10.075Z,WMS,1.1.1,GetMap,"/geosolutions/wmsLAYERS=geosolutions:ccounties&STYLES=&FORMAT=image/png&SERVICE=WMS&VERSION=1.1.1&REQUEST=GetMap&SRS=EPSG:4269&BBOX=-105.84010229493,39.543136352537,-105.34572204591,39.825884155271&WIDTH=577&HEIGHT=330",image/png,45,18692,success
	

