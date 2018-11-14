## Immigrant New York: introduction to mapping with QGIS
Fall 2018

In this workshop, you will learn introductory skills involved in using open source geographic information software (QGIS) to map existing spatial datasets. After the completion of this exercise, you should...

* have familiarity with the QGIS user interface
* comprehend the components of a shapefile
* have familiarity with the difference between vector and raster datasets
* design a compelling map composition
* perform a table join to add additional data to an existing shapefile’s attribute table
* query a GIS dataset, using both tabular and spatial queries

### Downloads
* Download the 1910 [Social Map of the Lower East Side](http://maps.nypl.org/warper/maps/16655#Show_tab)
* Download the vector data for this tutorial contained in the `data.zip` file.


### Historic Maps and Census data
#### Premise
We have found a historic map of social institutions on New York's Lower East Side in 1910 through the New York Public Library's [Map Warper](http://maps.nypl.org/warper/) project. We are interested in contextualizing the spatial organization of these institutions using historic census data from the same year available from the [National Historic Geographic Information System (NHGIS)](https://www.nhgis.org/).

Further catalog information about the 'Social Map of the Lower East Side' is available [here](https://digitalcollections.nypl.org/items/8ae8bd90-f78e-0130-dc5a-58d385a7b928).

Specifically we are interested in learning:
* In 1910 New York what neighborhoods had the highest concentrations of population?
* In 1910 New York what neighborhoods had the highest concentrations of foreign born residents?
* How do these patterns relate to the organization of social institutions represented by the 1910 New York Times map?

#### To begin
1. **Launch** QGIS. Your new blank map project will look like this:

![blank](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MappingData01/01_OpenQGIS.png)

2. Begin to familiarize yourself with the interface. You can also refer to this [brief description](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Resources/QGIS_InterfaceDescription.md) of the elements of the interface for more information.

#### Adding Layers
We will add two datasets to our map project to begin:
* a **vector** dataset of Race/Nativity by census tract from the 1910 Census
* a georeferenced **raster** dataset of the 1910 Social New York map ("georeferenced" means that the map that has been given geographic coordinates, it knows where it is in the world)

3. Add the 1910 Race/Nativity census data using the `Add Vector Layer` button.

  ![vector](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MappingData01/02_Adding_Layers_Vector.png)

4. **Navigate** to the 01_ImmigrantNewYork_data/Shape/nyc_tract_1910 folder. You'll notice a number of different file extensions that are likely unfamiliar. All five files in the folder which have the name `nyc_tract_1910_StatePlane` but different file extensions are  components of the admin_0_countries shapefile, and the ones outlined in magenta are all elements of the populated_places shapefile. It is very important that all of these files stay together in the same folder otherwise QGIS will not be able to load the layer.

      * .shp - The main file that stores the feature geometry (required).
      * .shx - The index file that stores the index of the feature geometry (required).
      * .dbf - The dBASE table that stores the attribute information of features (required).
      * .sbn and .sbx - The files that store the spatial index of features.
      * .prj - The file that stores the coordinate system information.
      * For more information on these extensions and others see [this explanation by ESRI](http://webhelp.esri.com/arcgisdesktop/9.2/index.cfm?TopicName=Shapefile_file_extensions).

5. Add the Social Map of the Lower East Side using the `Add Raster Layer` button.

**Save your project**

6. Change the drawing order of your layers in the layers panel. The order of the layers in the layers panel determines the order they are drawn in. Move the 1910 Census layer on top of the Social New York Map.

7. Change the symbology of the 1910 Census layer:
      * right click on the 1910 Census layer in the layers panel and open the `Layer Properties` menu.
      * Select the `Style` tab.
      * Select `Simple Fill`
      * In the Symbol Layer Type drop down menu select `Outline: Simple Line`
      * Click `OK`

**Save your project**

#### Understanding the contents of GIS data
All vector GIS data consists of two types of information: geometric information, and attribute information. We have been viewing the geometric information in the Map View. Lets take a look at the attribute information.

8. **Open the 1910 Census attribute table**: right click on the 1910 Census layer in the layers panel and select `Open Attribute Table`. A table of values will appear.
9. Inspect the column headings. At the far right you'll notice several columns named with alphanumeric codes. These correspond with codes that are made available in the codebook that accompanies the data when we download it from NHGIS.
10. Open the codebook to decipher what information is contained in each of the far right columns
11. **Calculate a new field to create a total population count.** Select the `Open Field Calculator` button (looks like an abacus)
      * Select `Create New Field`
      * Set the `Output field name` as "PopTotal"
      * In the expression builder window construct the following expression ` "A7D001"  +  "A7D002"  +  "A7D003"  +  "A7D004"  +  "A7D005" `
12. Select the top 5 most populous census tracts, they will appear highlighted in yellow in the Map View.
      * Where are they located?

**Save your project**

#### Symbolizing Quantitative Data
Next we will create a thematic map of 1910 Census information which shows differences in the number of foreign born whites by census tract.

13. Right click on the 1910 Census layer in the layers panel and open the `Layer Properties` menu.
14. Select the `Style` tab.
15. From the top drop down menu choose `Graduated`
16. In the Column menu, select the column which contains the population of foreign born white individuals (A7D003)
17. Choose a pleasing color ramp
18. For the mode select `Natural Breaks (Jenks)`
19. Click `Apply` and then `OK`

**Save your project**

***Go further*** Experiment with different classification modes and numbers of classes, or create your own manual breaks in the data. What argument does each option convey?

#### Calculating New Fields
Which census tract has the highest population density in New York in 1910? Which census tracts have a population density of greater than XXXX?
We will use the field calculator, exploiting the fact that we can determine the area of each of our census tracts (recall one of the two core qualities of GIS data is that it has geometric information), and then selection methods to answer these questions.
20. Open the `Project Properties` and in the `General` tab, and select `Acres` for the `Units for Area Management`
21. Open the attribute table of the 1910 Census layer (right click on its name in the layers panel, and select open attribute table)
22. Select the `Field Calculator`
23. Select `Create New Field`
24. Set the `Output field name` as "PopDens"
25. Set the `Output field type` to `Decimal (real)`
26. Set the `Precision` to 2 or 3
27. In the expression builder window construct the following expression: ` "PopTotal" / $area `
      * the middle menu contains all of the fields and values that you can use in the expression building. If you expand the geometry window you will see the option for `$area`. This variable returns the area of the current feature using the units you set in the project properties menu.

**Save your project**

#### Data Querying, by Location and attributes
How many census tracts have a population density of greater than 500 residents per acre?
28. Open the attribute table for the 1910 Census tracts if it isn't already
29. Click the `Select features using an expression` button (yellow square with an E)
30. In the expression builder construct the following expression: ` "PopDens" > 500`
31. Click `Select`
The selected features will appear in yellow on your map and will be highlighted in the attribute table. The top bar of the attribute table will also tell you how many are selected.

***In how many census tracts are there more than 500 residents per acre?***
***Go further:*** what other kinds of section expressions can you think of? Ask and answer several spatial question about this dataset.

#### Designing your map composition and exporting
In order to turn our maps into files which can be saved as PDF or image files we will use the Print Composer. The print composer is a somewhat frustrating, but nevertheless powerful graphics editor. It is not at all intuitive, but the best way to learn how to use it is to explore the various menus.

32. Create a new print composer. When prompted you can either name the print composer or not.
![Attribute](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MappingData01/25_PrintComposer.png)

33. **Add a new map to the composer**: select the add new map button.
34. Click once to begin to drag a rectangle over the area on the page that you would like the map to occupy and click again to stop.
    * *Note: Whatever is showing in your QGIS map project window as you create this new map in the print composer is what will appear in the new map.*
![Attribute](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MappingData01/27_PrintComp.png)

35. **Add a legend**. Select `Add new legend`, and again click to draw a rectangle where you would like to place the legend. An unformatted legend that matches the information from the Layers panel will appear.
36. You can use the options in the Item Properties tab to change which layers are represented in the legend and to change the labeling of the layers in the legend. Scroll within this tab to familiarize yourself with which properties about the legend you can change.
37. Format the legend and change the titles of each dataset so they are more descriptive. To do this un-click “Auto update” to make changes, then change the layer names by clicking the “legend item properties”
38. Use one of the export options at the top menu bar to save the map composition as an image file, PDF, or SVG.


### Preparing Data Downloaded from NHGIS
For the purposes of this workshop data downloaded from NHGIS was pre-cleaned and processed. The following provides some basic instructions for downloading and cleaning data from NHGIS in order to work with it in QGIS.

1. Use NHGIS to select the tables and years your are interested in. You will need to register for an account.
2. Download both the tabular information and the boundary files for the years you are interested.
3. Open a new QGIS project and add both the geometric and tabular data to your project.
4. Use manual selection to select the polygons you are interested in (for example for 1910, the census tract file includes tracts for several US cities not just New York, so I would zoom and pan to New York and use the `select features by area` button to draw a rectangle around New York).
5. Export the selected features as a new layer.
      * Right click on the layer name in the layers panel.
      * Select `Save as`
      * Choose a location to save your new shapefile and name it
      * Be sure to select `Save only selected features`
      * Select `Add saved file to map` if it isn't already
6. Open the attribute table of your new feature class, you'll notice that there are not any fields which contain the census counts information we downloaded from NHGIS. This is because these values were downloaded separately as a table. We will need to perform an operation called a **table join** to attach them to the geometry of our shape file

#### Joining New Tabular Data with a Table Join
To add further information to the attribute table of a vector GIS dataset we can perform an operation called a table join. A table join allows GIS users to combine tabular data with vector data based on an ***identical*** field in their attribute tables.
1. Add a delimited the text layer to your project that we downloaded from NHGIS.
2. Open the Layer Properties menu of the shapefile of census tracts you aim to join tabular information to. (in this case it is the NYC tracts shapefile we just created). We always start the join on the file that we are joining to.
3. Navigate to `Joins` in the left hand menu. Click the `+` icon.
4. **Select** the delimited text layer from NHGIS as the “join layer”, for NHGIS data GISJOIN is always the “join field” as well as the "target field" (i.e. the field that is unique and identical and shared by the tabular data and the shapefile we are joining to, note the name of this field does not need to be the same but the values in it must be).
5. Select the box next to “Custom field name prefix” and delete the contents of that field (note this is a helpful field if we are joining data from many different tables to one shapefile as it allows you to distinguish the source table). **Click** `OK` to close the join dialog. Then **Click** `OK` to close the layer properties menu.

6. Open the attribute table of your tracts shape file -- you should see that new fields were added.
7. Joins are temporary relationships, we have not altered the underlying data at all. In order to make this permanent we need to save the layer as a new shapefile.

You have now completed the table join.

**Save your project**

#### Carrying out a Spatial Join
Another way of joining and sumamrizing data is by location. This is a powerful function of a GIS and opens up exciting possibilities for working with spatial data across different formats. A spatial join allows a user to join and summarize data between different shapefiles and geometries based on how they intersect, contain one another, etc.

We are going to explore the Emigrant Savings Bank records that have been geocoded by the NYPL. These data are in point format, one point for every record - or address - associated with a historical bank transaction. They contain name, address and loan amount info, in the case of the data prepared for this tutorial. But there's more info available in the full dataset, including building material type and stories. You can explore all available data in the `csv` provided.

1. Add the `emigrantCity_point_StatePlane.shp` file as a layer
2. On the QGIS menu, navigate to `Vector / Data Management / Join attributes by location`
3. In the `Join attributes by location` menu:
  * Set the `Target vector layer` to your census tract file (polygons)
  * Set the `Join vector layer` to the Emigrant savings bank points layer
  * For `Geometric predicate` select `contains`
  * For `Attribute summary` toggle to `Take Summary of Intersecting Features`. This will produce summary statistics for the join, such as the total amount of loans given to people in the entire Census Tract, the mean loan amount per tract, and the number of total loans given, as documented and transcibed in this database
  * Leave the `Statistics for Summary` field as `sum, mean, min, max, median`. For every summary field that it can, this operation will produce those statistics. You can customize this as relevant to your research questions
  * Click `Run`. QGIS will open the output layer in your map frame, at which point you can explore the attribute table, save it as a shapefile, and visualize the results of summarizing the `LoanAmount` field by location. 
  
Where were the most loans given out by the Emigrant Savings Bank (mode)? Which Census Tract received the most funds (max summary LoanAmount)? 

**Save your project**

#### Creating your Own Shapefile and Digitizing
You can use QGIS to create your own shapefile or spatial dataset. This is useful for digitizing features on historical basemaps that you'd like to bring into your analysis. Using the 1910 Social Map of the Lower East Side as your resource, you will learn how to make a point file that captures and maps the different types of institutions on the Lower East Side.

1. Organize and reorder the layers in the `Layers Panel` so that you are just looking at the 1910 Social Map. 
2. On the QGIS menu select `Create New Shapefile Layer`
3. In the `New Shapefile Layer` dialog box, you have the option to create a `point`, `line`, or `polygon` layer. For this task, we're going to select `point`, but if you were outling builing footprints, or tracing streets or pathways, you would want to select a relevant geometry
4. As mentioned earlier, all Shapefiles have a projection: a system or set of rules for transforming the data so that it can be lined up and mapped in a GIS. Your QGIS project has a default projection, and each layer has it's own projection stored in its constituent .prj file. For this tutorial, you will want to select a projection that matches the other data you're working with. That's NAD 1983 State Plane for New York / Long Island.
5. Now take a look at the basemap and its legend and consider: what kind of information are we trying to capture? We need to structure the attributes of our shapefile so that as we move over the map, drawing or plotting points based on marked locations of instutions, we can capture the type or category of institution in the attributes of our file. We'll need to add at least one field to the attributes of our new Shapefile to do so.
6. Under **New Field**
    * Enter `type` into the `Name` field
    * Select `Text Data` and maximum length for text entries (80, the default, is fine for now)
    * Click `Add to fields list`. You should see the new field appear in the list below.

7. Click `OK`. 
8. Your will have to name your file and specify the file path to where you would like to save it
9. Click `Save`. You new Shapefile will be added to the map and `Layers Panel`.
10. Click once on the file you just created in the `Layers Panel`
11. Click the `Toggle Editing` button on the QGIS menu
12. You have now turned on editing for your point file and can add points
13. Click the `Add Feature` button
14. Mouse over to a place on the 1910 Social Map where you'd like to add a point feature (based on where an institution is depcited)
15. Click on where you'd like to add a new point feature with the editing cursor
16. A dialog box will open, asking you to fill in the `type` field you created earlier. With CAPS LOCK on, enter the type of social institution based on the legend. You can leace the `ID` field blank. 
17. Hit `enter/return` or click `OK`
18. To stop editing, click the `Toggle Editing` button again and clik `Save` to save all of your changes.

You can change the style of this point layer to reflect the different types of institutions on the map:
19. Right click on your point layer in the `Layers Panel`
20. Open the `Properties` menu
21. Under the `Style` tab, select `Categorized` on the drop down menu
22. Under the `Column` drop down, select `type` and click the `Classify` button below to assign a new color to each point by `type`
23. Click `Apply` and `Ok` to commit these changes and exit the menu

**Save your project**

**Go further** You could now imagine joining these to block or census tracts by location to see how the distribution of certain types of institutions compares surrounding demographics. 
