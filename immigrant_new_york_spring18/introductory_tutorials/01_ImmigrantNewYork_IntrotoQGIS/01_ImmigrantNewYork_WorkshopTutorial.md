##Immigrant New York: introduction to mapping with QGIS
Spring 2018

In this workshop, you will learn introductory skills involved in using open source geographic information software (QGIS) to map existing spatial datasets. After the completion of this exercise, you should...

* have familiarity with the QGIS user interface
* comprehend the components of a shapefile
* have familiarity with the difference between vector and raster datasets
* design a compelling map composition
* perform a table join to add additional data to an existing shapefile’s attribute table
* query a GIS dataset, using both tabular and spatial queries

### Downloads
Download the GitHub repository for this workshop. Using the green button [here](https://github.com/CenterForSpatialResearch/gis_resources), select `Download ZIP`.
The data we will use for this exercise is located in the `01_ImmigrantNewYork_data` folder.

### Historic Maps and Census data
#### Premise
We have found a historic map of social institutions on New York's Lower East Side in 1910 through the New York Public Library's [Map Warper](http://maps.nypl.org/warper/) project. We are interested in contextualizing the spatial organization of these institutions using historic census data from the same year available from the [National Historic Geographic Information System (NHGIS)](https://www.nhgis.org/).

Specifically we are interested in learning:
* In 1910 New York what neighborhoods had the highest concentrations of population?
* In 1910 New York what neighborhoods had the highest concentrations of foreign born residents?
* How do these patterns relate to the organization of social institutions represented by the 1910 New York Times map?

![1910 NY Times map](https://github.com/CenterForSpatialResearch/gis_resources/tree/master/immigrant_new_york_spring18/introductory_tutorials/img/Socialmap.jpg)

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

4. **Navigate** to the 01_ImmigrantNewYork_data/Shape/nyc_tract_1910 folder. You'll notice a number of different file extensions that are likely unfamiliar. All five files in the folder which have the name `nyc_tract_1910_StatePlane_joincensus` but different file extensions are  components of the admin_0_countries shapefile, and the ones outlined in magenta are all elements of the populated_places shapefile. It is very important that all of these files stay together in the same folder otherwise QGIS will not be able to load the layer.

* .shp - The main file that stores the feature geometry (required).
* .shx - The index file that stores the index of the feature geometry (required).
* .dbf - The dBASE table that stores the attribute information of features (required).
* .sbn and .sbx - The files that store the spatial index of features.
* .prj - The file that stores the coordinate system information.
* For more information on these extensions and others see [this explanation by ESRI](http://webhelp.esri.com/arcgisdesktop/9.2/index.cfm?TopicName=Shapefile_file_extensions).

5. Add the Social Map of the Lower East Side using the `Add Raster Layer` button.

6. Change the drawing order of your layers in the layers panel. The order of the layers in the layers panel determines the order they are drawn in. Move the 1910 Census layer on top of the Social New York Map.

6. Change the symbology of the 1910 Census layer:
  * right click on the 1910 Census layer in the layers panel and open the `Layer Properties` menu.
  * Select the `Style` tab.
  * Select `Simple Fill`
  * In the Symbol Layer Type drop down menu select `Outline: Simple Line`
  * Click `OK`

#### Understanding the contents of GIS data
All vector GIS data consists of two types of information: geometric information, and attribute information. We have been viewing the geometric information in the Map View. Lets take a look at the attribute information.

7. **Open the 1910 Census attribute table**: right click on the 1910 Census layer in the layers panel and select `Open Attribute Table`. A table of values will appear.
8. Inspect the column headings. At the far right you'll notice several columns named with alphanumeric codes. These correspond with codes that are made available in the codebook that accompanies the data when we download it from NHGIS.
9. Open the codebook to decipher what information is contained in each of the far right columns
10. **Calculate a new field to create a total population count.** Select the `Open Field Calculator` button (looks like an abacus)
  * Select `Create New Field`
  * Set the `Output field name` as "PopTotal"
  * In the expression builder window construct the following expression ` "A7D001"  +  "A7D002"  +  "A7D003"  +  "A7D004"  +  "A7D005" `
11. Select the top 5 most populous census tracts, they will appear highlighted in yellow in the Map View.
 * Where are they located?

#### Symbolizing Quantitative Data
Next we will create a thematic map of 1910 Census information which shows differences in the number of foreign born whites by census tract.

12. Right click on the 1910 Census layer in the layers panel and open the `Layer Properties` menu.
13. Select the `Style` tab.
14. From the top drop down menu choose `Graduated`
15. In the Column menu, select the column which contains the population of foreign born white individuals (A7D003)
16. Choose a pleasing color ramp
17. For the mode select `Natural Breaks (Jenks)`
18. Click `Apply` and then `OK`

***Go further*** Experiment with different classification modes and numbers of classes, or create your own manual breaks in the data. What argument does each option convey?

#### Data Querying, by Location and attributes
19.




#### Designing your map composition and exporting
In order to turn our maps into files which can be saved as PDF or image files we will use the Print Composer. The print composer is a somewhat frustrating, but nevertheless powerful graphics editor. It is not at all intuitive, but the best way to learn how to use it is to explore the various menus.

* Create a new print composer. When prompted you can either name the print composer or not.
![Attribute](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MappingData01/25_PrintComposer.png)

* **Add a new map to the composer**: select the add new map button.
* Click once to begin to drag a rectangle over the area on the page that you would like the map to occupy and click again to stop.
    * *Note: Whatever is showing in your QGIS map project window as you create this new map in the print composer is what will appear in the new map.*
![Attribute](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MappingData01/27_PrintComp.png)

* **Add a legend**. Select `Add new legend`, and again click to draw a rectangle where you would like to place the legend. An unformatted legend that matches the information from the Layers panel will appear.
* You can use the options in the Item Properties tab to change which layers are represented in the legend and to change the labeling of the layers in the legend. Scroll within this tab to familiarize yourself with which properties about the legend you can change.
* Format the legend and change the titles of each dataset so they are more descriptive. To do this un-click “Auto update” to make changes, then change the layer names by clicking the “legend item properties”
* Use one of the export options at the top menu bar to save the map composition as an image file, PDF, or SVG.


### Preparing Data Downloaded from NHGIS
For the purposes of this workshop data downloaded from NHGIS was pre-cleaned and processed. The following provides some basic instructions for downloading and cleaning data from NHGIS in order to work with it in QGIS.


#### Joining New Tabular Data with a Table Join
To add further information to the attribute table of a vector GIS dataset we can perform an operation called a table join. A table join allows GIS users to combine tabular data with vector data based on an ***identical*** field in their attribute tables.

We will add further census information


We always start the join on the file that we are joining to. Here, we are joining the population estimates table to the country boundary shapefile. Thus, Open the Properties for admin_0_countries, and navigate to “Joins” in the left hand menu. Click the “+” icon.

ecause vector GIS data consists of both geometric information and attribute information we can add further
