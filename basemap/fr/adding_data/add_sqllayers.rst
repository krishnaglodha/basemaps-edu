.. module:: geoserver.add_sqllayers
   :synopsis: Apprenez à ajouter en SQL Parametric View based Layer.

.. _geoserver.add_sqllayers:

Ajouter en SQL Parametric View based Layer
------------------------------------------

La manière traditionnelle d'utiliser  des données sauvé dans des bases de données   est de configurer une table ou une vue de base de données comme un nouveau layer dans GeoServer. 
A partir de GeoServer 2.1.0, l'utilisateur peut également créer un nouveau layer en spécifiant une requête SQL brut, sans avoir à créer effectivement une vue dans la base de données. 
Le SQL peut également être paramétré, et les valeurs des paramètres adoptée en même temps qu'une demande WMS ou WFS.

**Création d'une vue SQL plaine**

#. Afin de créer une vue SQL, l'administrateur peut entrer dans le :guilabel:`Add a new resource` de la page :guilabel:`Layers`. 

   .. figure:: img/sqlviews_addlayer.png
      :width: 600
   
#. Lors de la sélection d'un database backed store une liste de tables et vues disponibles pour publication paraîtra, mais au fond de celui-ci un nouveau lien paraîtra, :guilabel:`Configure new SQL view`:

   .. figure:: img/sqlviews_postgrestore.png
      :width: 600
   
   .. figure:: img/sqlviews_addsqllayer.png
      :width: 600
   
#. En sélectionnant le lien :guilabel:`Configure new SQL view` ouvrira une nouvelle page où l'instruction SQL peut être spécifiée:

   .. figure:: img/sqlviews_plainsql_params.png
	  
	 Plain SQL View configuration

   .. code-block:: sql

    SELECT st.obs_year, 
				st.storm_num, 
				st.storm_name,
				min(st.obs_datetime)
				AS storm_start, max(st.obs_datetime)
				AS storm_end, max(st.wind)
				AS max_wind, st_makeline(st.geom) 
				AS the_route
    FROM ( SELECT storm_obs.storm_num, 
				storm_obs.storm_name,
				storm_obs.wind, 
				storm_obs.press, 
				storm_obs.obs_datetime, 
				date_part('year'::text, storm_obs.obs_datetime)
				AS obs_year, storm_obs.geom
           FROM storm_obs
          ORDER BY date_part('year'::text, storm_obs.obs_datetime),
					storm_obs.storm_num, 
					storm_obs.obs_datetime) st
    GROUP BY st.obs_year, st.storm_num, st.storm_name
    ORDER BY st.obs_year, st.storm_num

   .. note:: La requête peut être n'importe quelle instruction SQL qui peut être valablement exécuté dans le cadre d'une sous-requête dans la clause FROM, qui est ``select * from (<the sql view>) [as] vtable``. Cela est vrai pour la plupart des instructions SQL,  mais la syntaxe spécifique pourrait être nécessaire de faire appel sur une procédure stockée en fonction de la base de données
   .. figure:: img/sqlviews_plainsql_refresh.png
      :width: 600
   
   .. note:: GeoServer fera de son mieux pour trouver automatiquement le type de géométrie et lasrid native, mais ils doivent toujours être vérifiés une deuxième fois et finalement corrigé. En particulier ayant le droit SRID (spatial Identifiant de référence) est la clé pour avoir des requêtes spatiales fonctionnent réellement. Dans de nombreuses bases de données spatiales du SRID est égal au code EPSG pour le système de référence spatiale spécifique, mais ce n'est pas toujours le cas (par exemple, Oracle dispose d'un certain nombre de codes EPSG SRID non).
#. Spécifiez un SRID valide. 

   .. figure:: img/sqlviews_plainsql_refresh_srid.png

     Forcer manuellement 4326 SRID dans ce cas

#. Une fois la requête et les informations d'attributs sont définis appuyez :guilabel:`Save` et la page de configuration des nouveaux layers apparaîtra.  la page aura un lien vers un éditeur de vue SQL au bas de ``Data`` tab

   .. figure:: img/sqlviews_plainsql_featuretype.png
      :width: 600
	 
#. Assurez-vous que le CRS est ``EPSG:4326`` and **write manually** ``(-180,-90,180,90)`` des valeurs dans les sections :guilabel:`Bounding Boxes`. 

   .. figure:: img/sqlviews_plainsql_bbox.png
      :width: 600

#. Cliquez :guilabel:`Save`. 

A ce stade, le nouveau WMS layer est publié avec GeoServer.

**Création d'un paramétrique vue SQL**

   .. Attention:: En règle générale, utilisez les paramètres de substitution  SQL uniquement si la fonctionnalité requise ne peut être obtenue avec des moyens plus sûrs, telles que le filtrage dynamique (filtres CQL) ou la substitution de paramètres SLD. N'utilisez que des paramètres SQL comme un dernier recours, paramètres mal validées peuvent ouvrir la porte à des attaques par `injection SQL <http://en.wikipedia.org/wiki/SQL_injection>`_.

Une vue paramétrique SQL est basé sur une requête SQL contenant des paramètres dont les valeurs peuvent être dynamiquement fournit avec demandes WMS or WFS. Un paramètre est lié par des signes %, peut avoir une valeur par défaut, et doit toujours avoir une expression régulière de validation.

#. Afin de créer une vue paramétrique SQL exécutez les étapes 1 et 2 comme avant et puis insérez les paramètres suivants: 

   .. figure:: img/sqlviews_parametricsql_params.png
	  
	 Parametric SQL View configuration

   .. code-block:: sql

	SELECT date_part('year'::text, t1.obs_datetime) AS obs_year, t1.storm_num, t1.storm_name, t1.wind, t2.wind AS wind_end, t1.press, t2.press AS press_end, t1.obs_datetime, t2.obs_datetime AS obs_datetime_end, st_makeline(t1.geom, t2.geom) AS geom
	FROM storm_obs t1
	JOIN ( SELECT storm_obs.id, storm_obs.storm_num, storm_obs.storm_name, storm_obs.wind, storm_obs.press, storm_obs.obs_datetime, storm_obs.geom
		   FROM storm_obs) t2 ON (t1.obs_datetime + '06:00:00'::interval) = t2.obs_datetime AND t1.storm_name::text = t2.storm_name::text
	WHERE 
		date_part('year'::text, t1.obs_datetime) BETWEEN %MIN_OBS_YEAR% AND %MAX_OBS_YEAR%
	ORDER BY date_part('year'::text, t1.obs_datetime), t1.storm_num, t1.obs_datetime

   .. note:: La requête définit deux paramètres ``%MIN_OBS_YEAR%`` and ``%MAX_OBS_YEAR%``.

#. Cliquez :guilabel:`Guess parameters from SQL`. GeoServer créera automatiquement les champs avec les paramètres spécifiés dans la vue: 

   .. figure:: img/sqlviews_parametricsql_guess_params.png
      :width: 600
	  
   .. note:: Toujours fournir des valeurs par défaut pour chaque paramètre afin de laisser le layer fonctionner correctement et être aussi sûr que l'expression régulière pour la validation des valeurs sont correctes.


	Exemples d'expressions régulières:

		* ``^[\d\.\+-eE]+$`` va vérifier que la valeur du paramètre est composé des éléments valables pour un nombre à virgule flottante, finalement en notation scientifique, mais ne vérifie pas que la valeur fournie est en fait un nombre à virgule flottante valide
		* ``[^;']+`` va vérifier la valeur du paramètre ne contient pas de citations ou semicolumn, prévenir les attaques communes d'injection sql, sans réellement imposer beaucoup sur la structure de la valeur du paramètre

#. Remplissez quelques valeurs par défaut pour les paramètres, de sorte que GeoServer peut exécuter la requête et vérifier les résultats dans les prochaines étapes. Régler ``MAX_OBS_YEAR`` à 2020 et ``MIN_OBS_YEAR`` à 0.

#. :guilabel:`Actualiser` les attributs, consultez le SRID de la géométrie et de publier la couche comme avant.
   Attribuer également le style ``storm_track_interval`` au layer comme style par défaut.

   .. figure:: img/sqlviews_parametricsql_publishing.png
      :width: 600
   
#. Cliquez sur :guilabel:`OpenLayers` sur la liste :guilabel:`Layer Preview` pour :guilabel:`v_storm_interval` layer.

#. À première vue, vous ne verrez rien puisque le layer utilise les paramètres par défaut pour les années d'observation. Spécifiez deux ans pour la vue en ajoutant le paramètre à la fin de la Demande de GetMap:

   ``viewparams=MIN_OBS_YEAR:2000;MAX_OBS_YEAR:2000``

  Vous devriez obtenir une telle demande:
   
  .. code-block:: html

   http://localhost:8083/geoserver/geosolutions/wms?service=WMS&version=1.1.0&request=GetMap&layers=geosolutions:v_storm_track_interval&styles=&bbox=-180.0,-90.0,180.0,90.0&width=660&height=330&srs=EPSG:4326&format=application/openlayers&viewparams=MIN_OBS_YEAR:2000;MAX_OBS_YEAR:2000
   

Maintenant, vous êtes capable de voir les ouragans de la vue paramétrique et également choisir dynamiquement l'interval d'intérêt des années d'observation.

   .. figure:: img/sqlviews_parametricsql_preview.png
      
	 Parametric SQL View OL preview
