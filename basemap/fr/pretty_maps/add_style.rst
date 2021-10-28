.. module:: geoserver.add_style

.. _geoserver.add_style:


Ajouter un Style
----------------

La fonction la plus importante d'une carte serveur web est l'abilité de donner un style et de rendre led données. Cette séction explique comment ajouter un nouveau style à GeoServer et comment configurer le style de défaut pour une particulière couche.

#. De la GeoServer `Welcome Page <http://localhost:8083/geoserver>`_ naviguez jusq'à :guilabel:`Style`.

   .. figure:: img/style1.png
      :width: 600

      Naviguez jusq'à configuration du Style 
     
#. Cliquez :guilabel:`New`

   .. figure:: img/style2.png

     Ajouter un nouveau style

#. Tapez "manird" dans le :guilabel:`Name` domain et remarquez dans le fichier téléchargé le dialogue :guilabel:`SLD file`.

   .. figure:: img/style3.png

      Spécifiez le style du nom et peuplez d'un fichier.

#. Naviguez jusq'au workshop (en linux) :file:`${TRAINING_ROOT}/data/user_data/` répertoire (en windows :file:`%TRAINING_ROOT%\\data\\user_data\\`), selectionnez le :file:`foss4g_mainrd.sld` file, et cliquez :guilabel:`Upload`.


   .. note:: En GeoServer, les styles sont représenté via SLD (Styled Layer Descriptor) documents. SLD est un format XML pour spécifier la symbolization d'une couche. Quand un document SLD est téléchargé, ses contenus sont visibles dans l'éditeur de texte. L'éditeur peut etre utilisé pour modifier directement les contenus du SLD.

#. Ajoutez un nouveau style en cliquant :guilabel:`Submit`. Une fois sauvé vous devrez voire une chose pareille à la suivante:

   .. figure:: img/style4.png

      Présentation de style

#. Après avoir crèé le style, il faut l'appliquer à une couche vectorielle. Cliquez sur le lien :guilabel:`Layers`.

   .. figure:: img/style5.png
      :width: 600

      Naviguez jusq'aux Couches
     
#. Selectionnez le "Mainrd" sur la page `Layers`.

   .. figure:: img/style6.png

      Selectionnez une couche

#. Selectionnez l'étiquette :guilabel:`Publish`.

   .. figure:: img/style7.png

      Publiez l'étiquette

#. Assignez le nouveau style créè "mainrd" comme style de défaut .


   .. figure:: img/style8.png

      Publiez l'étiquette

   .. Attention:: Plusieurs nouveaux utilisateurs se méprennent entre le :guilabel:`Available Styles` et le :guilabel:`Default Style`, remarquez qu'ils sont différents, celui de défaut est utilisé quand le style n'est pas outrement spécifié dans une carte, les autres styles optionnels disponibles ne sont que des styles compatibles.

   .. note:: Geoserver 2.x assigne le style de défaut selon la géométrie et le type des objets, par ex: `ligne`, `poly`, `trame`, `point`.

#. Défilez jusq'au bout de la page et cliquez :guilabel:`Save`.

#. Utilisez l'avant-première de la carte pour voire comment le style s'approche et remarquez l'Echelle de dépendances
