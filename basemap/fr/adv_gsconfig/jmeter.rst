

.. _geoserver.jmeter:


Comment méasurer les performances avec JMeter
---------------------------------------------

Apache JMeter est une application Java desktop open source, faite pour pour charger le comportement fonctionnel de test et mesurer le rendement. Dans cette séctionnous allons à apprendre comment faire marcher le test de rendement en utilisant jMeter pour valuer les performances de GeoServer quand il sert requetes WMS .
Les tests de performance visent à stresser le serveur pour évaluer le comportement du temps de réponse et de débit avec un l'augmentation du nombre d'utilisateurs simulés envoyant des requetes simultané au serveur. 

.. note:: Idéalement, d'éviter de faire le serveur et pour lui faire des tests plus plausibles, JMeter devrait fonctionner sur une machine différente. 

#. Allez au répértoire ``/workshop/data/jakarta-jmeter-2.3.4/bin`` et démarrez le shell script **jmeter**:

   .. figure:: img/jmeter1.png
      :align: center
   
      *Running jMeter*

   .. figure:: img/jmeter2.png
      :align: center
   
      *jMeter interface*

#. Ajoutez un nouveau **Thread Group** avec le bouton de droite du souris sur ``Test Plan`` noeud de l'arbre:

   .. figure:: img/jmeter3.png
      :align: center
   
      *Adding a new ``Thread Group``*

#. Ajoutez un nouveau **Loop Controller** avec le bouton de droite du souris sur ``Thread Group`` noeud de l'arbre: 

   .. figure:: img/jmeter4.png
      :align: center
   
      *Adding a new ``Loop Controlle``*

#. Dans la fenetre ``Thread Group`` entrez le numéro de filetage pour le test (cela représente le nombre de demandes simultanées qui sont faites à GeoServer). Après chek ``Forever`` sur un domaine **Loop Count** la fréquence des secondes de la requete dans le domaine **Rump-Ip Period**. 

   .. figure:: img/jmeter14.png
      :align: center
   
      *Setting the ``Thread Group``*

#. Ajoutez un nouveau **HTTP Request** élément avec le bouton de droite du souris sur ``Loop Controller`` noeud de l'arbre: 

   .. figure:: img/jmeter5.png
      :align: center
   
      *Adding a new ``HTTP Request``*

#. Ajoutez les éléments suivants avec le bouton de droite du souris sur``Thread Group`` noeud de l'arbre:

   .. figure:: img/jmeter8.png
      :align: center
   
      *Adding a new HTTP Request Default*

   .. figure:: img/jmeter7.png
      :align: center
   
      *Adding a ``Listeners``*

#. Dans le **HTTP Request Default** entrez la configuration suivante:

   .. figure:: img/jmeter9.png
      :align: center
   
      *``HTTP Request Default`` configuration*

À ce stade jMeter est configuré pour éfféctuer à GeoServer un test de performance :

#. Selectionnez **Thread Group** sur le noeud de l'arbre et après ça cliquez sur ``Run`` et selectionnez **Start** pour démarrer jMeter test.

   .. figure:: img/jmeter13.png
      :align: center
   
      *sterting jMeter test*

#. Selectionnez ``View Results Tree`` pour voir directement les informations de demande produits et le résultat de la demande:
 
   .. figure:: img/jmeter15.png
      :align: center

      *``View Results Tree`` panel*
   
#. Selectionnez ``Aggregate Graph`` pour voir les informations statistiques sur les requêtes:
   
   .. figure:: img/jmeter19.png
      :align: center

      *``Aggregate Graph`` panel*

#. Selectionnez ``Spline Visualizer`` pour analyser l'évolution technique des requêtes:

   .. figure:: img/jmeter17.png
      :align: center

      *``Spline Visualizer`` panel*

#. Selectionnez ``Summary Report`` pour voir les donnèes d'intérêt principale relatives aux demandes en attente:
   
   .. figure:: img/jmeter18.png
      :align: center
  
      *``Summary Report`` panel*

   