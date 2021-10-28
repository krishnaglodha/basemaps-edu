.. module:: geoserver.crs_handling

.. _geoserver.crs_handling:


Coordonner Référence Handling System
------------------------------------

Cette séction décrit comment Coordonner Handling System (CRS) est géré en GeoServer, aussi bien que ce qui peut être fait pour prolonger GeoServer's CRS capacités de manipulation.

Coordonner Référence de Configuration du Système 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Quand on ajoute des données, GeoServer essaye à inspecter entêtes de données cherchant un code EPSG: si les données ont un CRS avec un explicite code EPSG et si toute la définition CRS derrière le code correspond à celle de GeoServer, le CRS sera déjà fixé pour les données.
si les données ont un CRS mais aucun code EPSG, il faudra deviner manuellement le code EPSG. Naviguer à `<www.spatialreference.org>`_ pourrait être une bonne option pour trouver l'exact code EPSG pour vos données.

Si on n'arrive pas à trouver un code EPSG, alors soit la donnée n'a pas CRS ou bien il n'est pas connu à GeoServer. Dans ce cas, il ya quelques options:

* Forcer le déclarée CRS, ignorant le natif.  C'est la meilleure solution si on sait que le natif CRS est faux.
* Reprojeter du natif au déclaré CRS. C'est la meilleure solution si le natif CRS est correct, mais ne peut pas être adapté à un numéro EPSG. Une alternative est d'ajouter un EPSG code personalisé qui correspond exactement au SRS natif.

Si vos données ne contiennent pas d'informations CRS natives, la seule option est de spécifier/forcer un code EPSG.

Definitions CRS Personnalisées
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Ajouter un CRS personnalisé
''''''''''''''''''''''''''''

Cet exemple montre comment ajouter une projection personnalisée en GeoServer.

#. Les paramètres de projéction doivent être fournies comme une définition WKT (texte bien connu). Le code ci-dessous est juste un exemple::

      PROJCS["NAD83 / Austin",
        GEOGCS["NAD83",
          DATUM["North_American_Datum_1983",
            SPHEROID["GRS 1980", 6378137.0, 298.257222101],
            TOWGS84[0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]],
          PRIMEM["Greenwich", 0.0],
          UNIT["degree", 0.017453292519943295],
          AXIS["Lon", EAST],
          AXIS["Lat", NORTH]],
        PROJECTION["Lambert_Conformal_Conic_2SP"],
        PARAMETER["central_meridian", -100.333333333333],
        PARAMETER["latitude_of_origin", 29.6666666666667],
        PARAMETER["standard_parallel_1", 31.883333333333297],
        PARAMETER["false_easting", 2296583.333333],
        PARAMETER["false_northing", 9842500.0],
        PARAMETER["standard_parallel_2", 30.1166666666667],
        UNIT["m", 1.0],
        AXIS["x", EAST],
        AXIS["y", NORTH],
        AUTHORITY["EPSG","100002"]]

   .. note:: Cet exemple de code a été formaté pour la lisibilité. L'information devra être fournie sur une seule ligne, ou avec barres obliques inversées à la fin de chaque ligne (excepté pour la dernière).

#. Allez au fichier :file:`user_projections` à l'intérieur de votre répertoire de données, et ouvrez le fichier :file:`epsg.properties`.  Si ce fichier n'existe pas, créez-le.

#. Inserez le code WKT pour la projéction à la fin du fichier (sur une seule ligne ou avec des barres obliques inverses)::

      100002=PROJCS["NAD83 / Austin", \
        GEOGCS["NAD83", \
          DATUM["North_American_Datum_1983", \
            SPHEROID["GRS 1980", 6378137.0, 298.257222101], \
            TOWGS84[0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]], \
          PRIMEM["Greenwich", 0.0], \
          UNIT["degree", 0.017453292519943295], \
          AXIS["Lon", EAST], \
          AXIS["Lat", NORTH]], \
        PROJECTION["Lambert_Conformal_Conic_2SP"], \
        PARAMETER["central_meridian", -100.333333333333], \
        PARAMETER["latitude_of_origin", 29.6666666666667], \
        PARAMETER["standard_parallel_1", 31.883333333333297], \
        PARAMETER["false_easting", 2296583.333333], \
        PARAMETER["false_northing", 9842500.0], \
        PARAMETER["standard_parallel_2", 30.1166666666667], \
        UNIT["m", 1.0], \
        AXIS["x", EAST], \
        AXIS["y", NORTH], \
        AUTHORITY["EPSG","100002"]]

.. note:: Notez le numéro qui précède le WKT.  Cela permettra de déterminer le code EPSG.  Donc, dans cet exemple, le code EPSG est 100002.

#. Sauvez le fichier.

#. Redémarrez GeoServer.

#. Verifiez que le CRS a été correctement analysée en naviguant jusq'à la page `srs_list` en `web_admin`.

#. Si la projection ne figurait pas, examinez les logs pour les erreurs.

Remplacer un code EPSG officiel
'''''''''''''''''''''''''''''''

Dans certaines situations, il est nécessaire de remplacer un code officiel EPSG avec une définition personnalisée. Un cas fréquent est la nécessité de changer les paramètres TOWGS84 afin d'obtenir une meilleure précision de reprojection dans des domaines spécifiques.

Le sous-système de référencement de GeoServer vérifie l'existence d'un autre fichier de propriétés :file:`epsg_overrides.properties`, dont le format est le même que :file:`epsg.properties`. Toute définition contenue dans :file:`epsg_overrides.properties` déterminerà **override** le code EPSG, pendant que les définitions stockées dans :file:`epsg.properties` ne peuvent que faire **add** à la base de données.

Des précautions particulières doivent être prises ors de la substitution des Datum paramètres, en particulièr les **TOWGS84** paramètres. Pour s'assurer que les paramètres substitués sont effectivement utilisés le code du Datum doit être enlevé, sinon le sous-système de référencement gardera la lecture de la base de données officielle à la recherche de la meilleure méthode de décalage du Datum (grille, 7 or 5 paramètres de transformation, transforme plaine affine).

Par example, si vous devez remplacer les paramètres officielles **TOWGS84** de EPSG:3003 afin de mieux correspondre à la zone péninsulaire de l'Italie::

  PROJCS["Monte Mario / Italy zone 1", 
  GEOGCS["Monte Mario", 
    DATUM["Monte Mario", 
      SPHEROID["International 1924", 6378388.0, 297.0, AUTHORITY["EPSG","7022"]], 
      TOWGS84[-50.2, -50.4, 84.8, -0.69, -2.012, 0.459, -5.791915759418465], 
      AUTHORITY["EPSG","6265"]], 
    PRIMEM["Greenwich", 0.0, AUTHORITY["EPSG","8901"]], 
    UNIT["degree", 0.017453292519943295], 
    AXIS["Geodetic longitude", EAST], 
    AXIS["Geodetic latitude", NORTH], 
    AUTHORITY["EPSG","4265"]], 
  PROJECTION["Transverse Mercator", AUTHORITY["EPSG","9807"]], 
  PARAMETER["central_meridian", 9.0], 
  PARAMETER["latitude_of_origin", 0.0], 
  PARAMETER["scale_factor", 0.9996], 
  PARAMETER["false_easting", 1500000.0], 
  PARAMETER["false_northing", 0.0], 
  UNIT["m", 1.0], 
  AXIS["Easting", EAST], 
  AXIS["Northing", NORTH], 
  AUTHORITY["EPSG","3003"]]
   
Vous devriez écrire ce qui suit (en une seule ligne, ici il est signalé formaté sur plusieurs lignes pour plus de lisibilité)::
  
  3003 =
   PROJCS["Monte Mario / Italy zone 1", 
  GEOGCS["Monte Mario", 
    DATUM["Monte Mario", 
      SPHEROID["International 1924", 6378388.0, 297.0, AUTHORITY["EPSG","7022"]], 
      TOWGS84[-104.1, -49.1, -9.9, 0.971, -2.917, 0.714, -11.68], 
      AUTHORITY["EPSG","6265"]], 
    PRIMEM["Greenwich", 0.0, AUTHORITY["EPSG","8901"]], 
    UNIT["degree", 0.017453292519943295], 
    AXIS["Geodetic longitude", EAST], 
    AXIS["Geodetic latitude", NORTH]], 
  PROJECTION["Transverse_Mercator"], 
  PARAMETER["central_meridian", 9.0], 
  PARAMETER["latitude_of_origin", 0.0], 
  PARAMETER["scale_factor", 0.9996], 
  PARAMETER["false_easting", 1500000.0], 
  PARAMETER["false_northing", 0.0], 
  UNIT["m", 1.0], 
  AXIS["Easting", EAST], 
  AXIS["Northing", NORTH], 
  AUTHORITY["EPSG","3003"]]

La définition a été modifiée à deux endroits, les paramètres **TOWGS84**, et le code du Datum, ``AUTHORITY["EPSG","4265"]``, a été retiré. 
