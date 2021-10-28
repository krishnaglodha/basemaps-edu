.. _geoserver.css:

Styling with CSS
----------------

The CSS extension module allows to build map styles using a compact, expressive styling language already well known to most web developers: Cascading Style Sheets.

The standard CSS language has been extended to allow for map filtering and managing all the details of a map production. In this section we'll experience creating a few simple styles with the CSS language.

Creating line styles
`````````````````````
#. From the main menu bar of Geoserver Admin, select the **Styles** entry

#. In the main styles page click on **Add a new style**

#. Assign ``css_mainrd`` as the style name

#. In the **Format** dropdown list select ``CSS`` instead of the SLD default value 

#. In the Style Editor, paste the following css code

   .. code-block:: css

     * { 
       stroke: orange;
       stroke-width: 6;
       stroke-linecap: round;
     }

#. Click on the *Apply* button and switch to the **Layer Preview** tab

#. Set the ``geosolutions:Mainrd`` as the preview layer
	 
   .. figure:: img/css_mainrd2.png
   
     The styled layer preview. After the first click on Apply, three new tabs will appear

#. Now let's create a cased line effect by adding a second set of colours and widths, and forcing two different z indexes for them. Edit CSS with code below and press Apply, then look at the map and see the generated SLD

    .. code-block:: css

       * { 
         stroke: orange, yellow;
         stroke-width: 6, 2;
         stroke-linecap: round;
         z-index: 1, 2;
       }

    .. figure:: img/css_mainrd_map2.png

#. Finally, let's add a label that follows the road, following same procedure as before

   .. code-block:: css

       * { 
         stroke: orange, yellow;
         stroke-width: 6, 2;
         stroke-linecap: round;
         z-index: 1, 2;
         label: [label_name];
         font-fill: black;
         font-family: Arial;
         font-size: 12;
         font-weight: bold;
         halo-color: white;
         halo-radius: 2;
         -gt-label-follow-line: true;
         -gt-label-group: true;
         -gt-label-repeat: 400;
         -gt-label-max-displacement: 50;
       }

   .. figure:: img/css_mainrd_map3.png

Creating point styles
`````````````````````
#. Similarly to the previous section, create a new CSS style called **css_bptlandmarks** and switch the Layer Preview to **bptlandmarks**

#. Insert the following in the CSS to get a basic point style, and press "Submit":
  
   .. code-block:: css


        * { 
          mark: symbol('circle');
          mark-size: 5;
        }

   .. figure:: img/css_point_map1.png

#. Let's change the color of the points by specifying a fill. If we specified a fill in the top level rule it would be interpreted as a polygonal fill, to express that we want to fill inside the marks we have to create a new rule with the ``:mark`` pseudo-selector:

   .. code-block:: css

        * { 
          mark: symbol('circle');
          mark-size: 5;
        }

        :mark {
          fill: cyan;
          stroke: darkblue;
        }

   .. figure:: img/css_point_map2.png

#. Finally, let's override the default styling for all shopping centers. Shopping centers are not easy to find, they have a ``MTFCC`` category of ``C3081`` and contain ``Shopping`` in the name

   .. code-block:: css

        * { 
          mark: symbol('circle');
          mark-size: 5;
        }

        :mark {
          fill: cyan;
          stroke: darkblue;
        }

        [MTFCC = 'C3081' AND FULLNAME LIKE '%Shopping%'] {
          mark: url("./img/landmarks/shop_supermarket.p.16.png");
          mark-size: 16;
        }

   .. figure:: img/css_point_map3.png

Creating polygon styles
```````````````````````
#. For this exercise, similarly to previous examples, create a new CSS style called **css_worldcountries** and change the current layer to **WorldCountries**

#. We want to create a simple 3 class thematic map based on the country population, stored in the ``POP_EST`` attribute

   .. code-block:: css


        [POP_EST < 10000000] { 
          fill: lightgrey; 
        }

        [POP_EST >= 10000000 AND POP_EST < 50000000] { 
          fill: olive; 
        }

        [POP_EST > 50000000] { 
          fill: salmon
        }

   .. figure:: img/css_poly_map1.png

#. Let's also add a very thin black border around all polygons, regardless of their population, using the ``*`` selector

    .. code-block:: css

        [POP_EST < 10000000] { 
          fill: lightgrey; 
        }

        [POP_EST >= 10000000 AND POP_EST < 50000000] { 
          fill: olive; 
        }

        [POP_EST > 50000000] { 
          fill: salmon
        }

        * {
          stroke: black;
          stroke-width: 0.2;
        }

   .. figure:: img/css_poly_map2.png


Styling raster data
```````````````````

#. For this exercise we are going to create a new ``css_raster`` style previewing it on the **srtm** layer

#. In order to activate raster styling the ``raster-channels`` property needs to be specified, in this case with a value of ``auto`` to let the engine choose automatically the bands to use:

   .. code-block:: css
   
      * { 
        raster-channels: auto;
      }

   .. figure:: img/css_ras_map1.png

#. The above map shows GeoServer automatically creating a grayscale map out of the elevation data with automatic adaptation to the current contents of the map (the black areas you see once applied the map are "no data" areas, try to go into an area that does not have any)

#. Now let's apply a color map to get a nicer and consistent looking map instead

   .. code-block:: css

        * { 
           raster-channels: auto;
           raster-color-map:
             color-map-entry(black, 0, 0)
             color-map-entry(green, 1500)

             color-map-entry(yellow, 2000)
             color-map-entry(maroon, 3000)
             color-map-entry(white, 4000);
        }

   .. figure:: img/css_ras_map2.png

