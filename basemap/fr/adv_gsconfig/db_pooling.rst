.. module:: geoserver.db_pooling

.. _geoserver.db_pooling:


Database Connection Pooling
---------------------------

Au moment de servir données à partir d'une base de données spatiales *connection pooling* est un aspect important pour obtenir de bonnes performances. Quand GeoServer sert une requête qui consiste à charger des données à partir d'une table de base de données, avant tout il faut établir une connexion avec la base de données. Cette connexion a un prix car il faut du temps pour la disposer.

L'objectif d'un connection pool est de maintenir la connexion d'une base de données sous-jacente entre les requêtes. L'avantage est que la connexion ne doit etre installée q'une fois à la première demande. Les requetes suivantes utilisent la connéction existente et réalisent un gain de performances en raison.

Quand un magasin de données soutenu par une base de données s'ajouté à GeoServer on crèe une connection pool à son intérieur. Cette connection pool est configurable.

options de la Connection pool 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

   .. list-table::
      :widths: 20 80

      * - max connections 
        - Est le nombre maximum de connexions que le pool peut contenir. Lorsque il est dépassée, les rerequêtes additionales qui nécessitent une connection avec la base de données seront arrêtées jusqu'à ce qu'une connexion du pool est disponible. Le nombre maximum de connexions limite le numéro de requetes simultanées qui peuvent etre posées à la base de données.
      * - min connections
        - Est le nombre minimum de connexions que le pool peut contenir. Il est tenu même quand il n'y a pas de demandes actives. Lorsque il est dépassée, à cause des requêtes du serveur, des connections supplémentaires s'ouvrent jusq'à que le pool rejoint sa taille maxime (décrite ci-dessus).
      * - validate connections
        - Fanion indiquant si les connections du pool devraient etre validées avant de les utiliser. Dans le pool une connexion peut devenir invalide pour plusieurs raisons y compris network breakdown, délai d'attente du serveur du database, etc..
          L'avantage de la fixation de ce fanion est q'on ne pourrait jamais utiliser une connexion invalide et cela peut éviter les erreurs des clients. L'inconvénient de la fixation de ce fanion est que la performance de la connexion est pénalisée afin de valider les connexions.
      * - fetch size
        - Le nombre des records lus du database change dans chaque réseau. S'il est réglé trop bas (<50) la latence du réseau aura une incidence négative sur la performance, S'il est réglé trop haut il peut consommer une partie importante de la mémoire de GeoServer et le pousser vers un erreur``Out Of Memory``. Par défaut 1000.
      * - connection timeout
        - Temps, en secondes, pendant lequel le connection pool attendrà avant d'abandonner sa tentative pour obtenir une nouvelle connexion du database. Par défaut 20 secondes. 
      * - max open prepared statements
        - Maximum numéro de déclarations préparées maintenues ouvertes et mise en cache pour chaque connexion dans le pool.
      * - max wait
        - numéro de secondes que la connéction va attendre avant de arrêter tenter d'obtenir une nouvelle connexion. Par défaut 20 secondes.
      * - validate connection
        - Vérifie que la connexion marche avant de l'utiliser.
   

