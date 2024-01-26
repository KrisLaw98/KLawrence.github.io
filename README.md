# GIS Portfolio

Welcome to my GIS portfolio. Below you can browse examples of my work compiled over the past year. 


## About me 

My name is Kristoff Lawrence, an Urban Planner with a passion for Geographic Information Systems (GIS). I am interested in how I can manipulate GIS software to solve or highlight social, geographical, and economic problems.<br />


graduate of The University of Technology (UTech), Jamaica's Urban and Regional Planning Bachelor's program (B.Sc. URP). During my 4 years at UTech, I was contracted by the National Environment & Planning Agency (NEPA) twice to work on their Development Order projects, which regulate and control land use.<br />
<br />

This realization is what ultimately led me to enroll in Michigan State University's Professional GIS Certificate program. As one of the country’s top-rated GIS certificate programs with a mission of training students to turn raw data into solutions for society’s most pressing problems, the perfect opportunity to develop the competencies I was seeking to practice those skills.<br />
<br />

You can view and download my resume [here](https://docs.google.com/gview?url=https://github.com/KrisLaw98/Resume/raw/07dce2cf2085f811da0a36203d419327e6870d2a/Kristoff%20Lawrence-Resume.pdf&embedded=true).

#### Jump to Section

[QGIS](#qgis)

- [Basic Network Visualization and Routing](#basic-network-visualization-and-routing) 
- [Creating a Landuse Map ](#creating-a-landuse-map ) 
- [Interpolating Point Data](#interpolating-point-data)
- [Locating the Nearest Facility with Origin-Destination Matrix](#locating-the-nearest-facility-with-origin-destination-matrix) 
- [Creating a Map From an Online Mass Shooting Database](#creating-a-map-from-an-online-mass-shooting-database) 
- [World Earthquakes](#world-earthquakes) 
- [Service Area Analysis using Openrouteservice](#service-area-analysis-using-openrouteservice)
- [Creating an Airport Location Map Using Python Scripting](#creating-an-airport-location-map-using-python-scripting) 
- [DEM of Crystal Mountain National Park, Gabon](#dem-of-crystal-mountain-national-park-gabon)

[ArcGIS](#arcgis)


- [American Bittern Migration](#american-bittern-migration)
- [Recruiting Patterns](#recruiting-patterns) 



# QGIS

## Basic Network Visualization and Routing

### Washington D.C Roadway Block
In this exercise, I used data from [Open Data DC](https://opendata.dc.gov/datasets/DCGIS::roadway-block/about) to create a visual representation of the road network, style one-way streets with arrows, and perform a shortest path analysis between two random points using QGIS. 

-First, I downloaded the Roadway_Block shapefile unzipped and loaded it into QGIS.

I then styled the one-way streets with arrows in the layer Styling Panel:
   - Choose Rule-based renderer.
   - Add a new rule for one-way streets using the "SUMMARYDIR" attribute using -> "SUMMARYDIR" in ('IB',  'OB').
   - Set the symbol layer type to Marker line.
   - Choose filled_arrowhead marker for one-way streets.
   - Implement a data-defined override for Rotation to align arrows correctly based on the one-way direction -> if( "SUMMARYDIR" = 'IB', 180, 0).

An image of the final product is displayed below.


![Layout 1](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/af8a6ac4-597b-42d3-9703-1030ddf1dfcc)

[Return to top](#jump-to-section)

## Creating a Landuse Map 

### Cape Town Land Use
In this exercise, I used data from [Cape Town Open Data Portal](https://opendata.dc.gov/datasets/DCGIS::roadway-block/about) using the [ArcGIS REST Services](https://citymaps.capetown.gov.za/agsext1/rest/services) to import the data from it to create a land use map for the CBD region of Cape Town.

-First, I access the data from the Cape Town Open Data Portal using the ArcGIS REST server URL to create a server connection, downloading three layers: Cape Town CBD layer, Zoning layer, and Split Zoning layer.

I then save all three layers locally as shapefiles (CBD, Zoning, and Split Zoning).

Open the Processing Toolbox to find and use the Intersection algorithm operation which extracts the overlapping portions of the Zoning and CBD layers, and the Split Zoning and CBD layers.

I had to fix geometries for all the layers using the Fix Geometries algorithm in the Processing Toolbox to address their invalid geometries.

Lastly, I styled the Zoning layer with Categorized based on zoning codes Value, merging each sub-category for better visualization.
   - Adjust symbology, colors, and labels for each zoning category.
   - Copy and paste the styling from the Zoning layer to the Split Zoning layer.

An image of the final product is displayed below.


![Layout](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/a7ed3fa2-5805-4771-a7c1-394ae53f85d6)


[Return to top](#jump-to-section)

## Interpolating Point Data

### Lake Arlington

In this exercise, I used data from the [Texas Water Development Board (TWDB)](https://www.twdb.texas.gov/surfacewater/surveys/completed/list/index.asp) to create an interpolated elevation surface of Lake Arlington, clip it to the lake boundary, generate contours, and add labels for elevation values. 

-First, I obtain the shapefiles for Lake Arlington's 2007-12 survey from the TWDB website. 

Perform TIN Interpolation:
   - Open the Processing Toolbox and find the TIN Interpolation tool.
   - Set Arlington_Soundings_2007_stpl83 as the Vector layer with Elevation as the Interpolation attribute.
   - Add Islands_2004_550_stpl83 as a break line with Elevation as the Interpolation attribute.
   - Set the extent using Boundary2004_550_stpl83.
   - Set pixel sizes to 5 in both X and Y dimensions.
   - Save the output as elevation_tin.tif and run the interpolation.

Clip Raster by Mask Layer:
   - Use the GDAL Clip raster by mask layer tool.
   - Set elevation_tin as the Input layer and Boundary2004_550_stpl83 as the Mask layer.
   - Save the output as elevation_tin_clipped.tif and run the tool.

Stylize Interpolated Raster:
   - Open the Layer styling panel for elevation_tin_clipped.
   - Set Symbology to Singleband pseudocolor, invert color ramp, and classify the values.
     
Generate Contours:
   - Use the GDAL Contour tool.
   - Set elevation_tin_clipped as the Input layer and specify a 5.000-foot interval for contour lines.
   - Save the output as contour.gpkg and run the tool.
     
Stylize Contour Layer:
    - Open the Layer styling panel for the contour layer.
    - Set symbology as needed.

Add Labels to Contours:
    - Switch to the Labels tab in the Layer styling panel.
    - Choose Single label, set the Value to ELEV, and adjust the placement as Curved.

An image of the final product is displayed below.


![Layout 1](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/31d6a49f-f129-4d36-b153-c820f279e412)


[Return to top](#jump-to-section)

## Locating the Nearest Facility with Origin-Destination Matrix

### Finding the Nearest Health Facility for Each Address

In this exercise, I used data from the [Open Data DC](https://opendata.dc.gov/) to create a map that you identifies the closest health facility to each address in Washington DC based on the calculated shortest path distances

-First, I obtained the shapefiles three shapefiles from Open Data DC that each contain the roadway block, address points, and community-based service provider respectively. 

- Load the Community_Based_Service_Providers.shp layer and filter it by entering the following query in the Filter Expression:
     ```
     "PROVIDER_T" IN ('Adult', 'Adult & Child')
     ```
   - Click Run.


[Return to top](#jump-to-section)

## Creating a Map From an Online Mass Shooting Database

### USA Mass Shootings (August 6, 1966 - May 6, 2023)

In this exercise, I used data from the [The Violence Project Mass Shooter Database](https://www.theviolenceproject.org/mass-shooter-database/) and [Homeland Infrastructure Foundation-Level Data (HIFLD) Open Data](https://hifld-geoplatform.opendata.arcgis.com/datasets/geoplatform::local-law-enforcement-locations/explore?filters=eyJUWVBFIjpbIkxPQ0FMIFBPTElDRSBERVBBUlRNRU5UIl19&location=26.839486%2C-96.938262%2C3.00) to create map showcasing mass shootings within the United States of America from August 1, 1966 to May 6, 2023. 

-First, I downloaded the first Excel spreadsheet with the mass shooting data from the database. I then Converted it to a .csv format and imported it to QGIS as a Delimited Text Layer. 

-Changed the symbology to heatmap and weigh points by number killed, changed blending mode to multiply, and changed radius to 8 mm. Changed the Label to Single Label and Value to Number Killed and rendered it to a scale of 1:4725400  so it shows when zoomed at that scale.

-Filtered the data from the Local Law Enforcement Locations feature class/shapefile and specifically downloaded the Local police department Locations from Homeland Infrastructure Foundation-Level Data (HIFLD) as an Excel spreadsheet. Convert to a .csv format and import into QGIS as a Delimited Text Layer but with a point coordinate geometry definition and change symbol blending mode to multiply and size to .4 mm.

-I also downloaded the Excel spreadsheet with mass shooting community-level data that contains demographic information. Convert to .csv format and then import to QGIS as Delimited Text Layer. Filter this new layer for 1 or more gun stores in zip code -> "N Gun Stores in Zip Code" >= 1.

-Execute the Join Attributes by Field Value algorithm. The first Input Layer is the regular mass shooting data with Table Field set to "total firearms brought to the scene".
The second input layer is the mass shooting community data with the Table Field data as "N of gun stores in zip code". Check the box with "discard records which could not be joined".

-In the new joined layer I changed the symbology to Graduated with "N Gun Stores in Zip Code" as the Value. The Mode is "Fixed Interval" with interval size set to 1 and with Classes set to 4.

Turn on Labels for the regular mass shooting data layer and set it to Single Labels with the Value as Number Killed to display this specific data (in green) at the areas where the shootings occurred.


Display

-The heatmap I used for the symbology does not display in the legend for the print layout. I researched a way to do this and learned about the Heatmap (Kernel Density Estimation) tool. I used the mass shooting database layer as the point layer, left everything to default, and then ran it.

-A new heatmap raster was created and then I edited the symbology –> I set the render type to singleband pseudocolor with the Min and Max set to the lowest no. of people killed (4) and most no. of people killed (60) in mass shootings up to that date respectively. I set the Mode to "equal interval" and put a minimum of 19 classes to create the makeshift heatmap for the legend in the print layout.



Images of the final product are displayed below.


![completed NA Mland](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/709d9f79-5ff8-4487-bc99-25095fecdc5c)

![completed Alaska and Hawaii](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/67561c2a-5f87-400e-8821-2c10979b1ee6)


[Return to top](#jump-to-section)

## World Earthquakes

### Earthquake Hazard Frequency and Distribution (August 27 - October 27, 2023)

In this exercise, I used data from the [United States Geological Survey (USGS)](https://earthquake.usgs.gov/earthquakes/map/?extent=-13.06878,-201.62109&extent=73.07384,22.5&sort=oldest) and [Socioeconomic Data and Applications Center (SEDAC)](https://sedac.ciesin.columbia.edu/) to create map showcasing world earthquakes with a magnitude between 5 - 8, and their hazard frequency and distribution from August 27, 2023 to October 27, 2023. 

-First, I searched for historical earthquake data from the USGS website.

-Select criteria (date- August 27 to October 27, 2023, magnitude between 5 and 8), download the Excel file with the data and convert it, and import it into QGIS as a delimited text layer.
   
-Change the symbology to Categorized with 3 classes with the Mode being Equal Interval and the Value being "magnitude".

-Now change the symbology to Rule-Based. It will automatically create the rules for each label based on the previous selections in the previous step.

-Turn on the labels for this layer and set the Value to "magnitude" to display the magnitude at each earthquake location.

-Within the browser, make a connection to the Web Map Service (WMS) Socioeconomic Data and Applications Center (SEDAC).

-Select the Earthquake Hazard Frequency and Distribution layer.

-Change the symbology of this Earthquake Hazard Frequency and Distribution layer to slightly transparent.


An image of the final product is displayed below.


![Earthquake HF D](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/01a51985-37f1-4226-b82b-afe6b1184cca)



[Return to top](#jump-to-section)

## Service Area Analysis using Openrouteservice

### Kochi Metro Rail Limited Metro System Service-Area Analysis

In this exercise, I used data from the [Kochi Metro Rail Limited (KMRL) Open Data](https://kochimetro.org/open-data/) to create map showcasing the service-area analysis of the Kochi Metro Rail Limited Metro System using Openrouteservice (ORS).

-First, download the metro rail station data for Kochi, India in GTFS format from Kochi Metro Rail Limited.

-Use QGIS Data Source Manager to import shapes.txt and stops.txt files from the GTFS data.
   - Convert the points in the shapes layer to a line layer representing the path of the metro line using the Points to Path tool.

-Use the ORS Tools Isochrones From Point-Layer tool for network analysis.
   - Select openrouteservice as the provider, stops as the input point layer, and specify parameters like travel mode (foot-walking and cycling) and time dimension (15 minutes).
   - Set the Location Type to "start" which treats the location(s) as starting point, and the destination as the goal.
   - Run the tool to generate isochrones representing areas accessible within a 15-minute walk and cycle from each metro station.

-Add the OpenStreetMap basemap to the canvas to visualize the results in the context of the road network.

An image of the final product is displayed below.


![Service Area Analysis using Openrouteservice](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/eb5660ee-f341-4ac6-bd3d-d38a5d0c7dea)


[Return to top](#jump-to-section)

## Creating an Airport Location Map Using Python Scripting

In this exercise, I used data from the [Natural Earth](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/airports/) and python scripting to create a map showing the locations of airports around the world.

-First, download the airport shapefile data from Natural Earth

Then:

**Access the Python Console:**
  - Open the Python Console by going to `Plugins` -> `Python Console`.

#### Python Scripting Steps

- **Get Reference to Active Layer:**
  ```python
  layer = iface.activeLayer()
  ```

- **Explore Layer Functions:**
  ```python
  dir(layer)
  ```

- **Iterate Through Features and Print Attributes:**
  ```python
  for f in layer.getFeatures():
    print(f['name'], f['iata_code'])
  ```

- **Print Coordinates of Each Feature:**
  ```python
  for f in layer.getFeatures():
    geom = f.geometry()
    print(geom.asPoint())
  ```

- **Print Name, Code, Latitude, and Longitude:**
  ```python
  for f in layer.getFeatures():
    geom = f.geometry()
    print('{},{},{:.2f},{:.2f}'.format(f['name'], f['iata_code'], geom.asPoint().y(), geom.asPoint().x()))
  ```

- **Write Data to a Text File:**
  ```python
  with open('/path/to/your/output/airports.txt', 'w') as file:
    for f in layer.getFeatures():
      geom = f.geometry()
      line = '{},{},{:.2f},{:.2f}\n'.format(f['name'], f['iata_code'], geom.asPoint().y(), geom.asPoint().x())
      file.write(line)
  ```

See images of the final product below.

![Airports notepad output](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/403a48e0-3ad3-4438-8507-96cf755b8563)

![airports](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/1cac44d7-905f-4874-a2ec-939ecd64a566)



[Return to top](#jump-to-section)

## DEM of Crystal Mountain National Park, Gabon

In this exercise, I used data from the [United States Geological Survey(USGS)](https://earthexplorer.usgs.gov/download/options/srtm_v3/SRTM1N00E010V3/) to create a map showcasing the DEM of the Crystal Mountain National Park in Gabon.


-First, download the GEOTIFF raster Layer from EarthExplorer-USGS.

-Then select `Processing Toolbox` -> `Contour Polygons`

-Select the Raster image as the input layer. Change the Interval between contour lines to 100, and within advanced parameters check the box that says "produce 3D vector". Press "Run".

-Edit the symbology for the new contour layer by adding the Draw Effect called Drop Shadow, setting it to 1.5.
   
-Change the stroke width to hairline. 

-Change symbology to Graduated with the value set to Max Elevation. Make it 20 classes with Equal Interval mode.

See the final product below.

![DEM(without contour line layer)](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/9ed67a13-bb44-44ad-aa40-6371628194ec)


[Return to top](#jump-to-section)

# ArcGIS


## American Bittern Migration

In this exercise, I used animal tracking data from the [Movebank](https://www.movebank.org/cms/webapp?gwt_fragment=page=studies,path=study438644854) to create a map showcasing the migration pattern of twenty adult male American Bitterns (Botaurus lentiginosus) between July 13, 1998 to July 23, 2003.

-First, download data from Movebank.org as an ESRI shapefile.
 Import the files to the geodatabase within the Catalog pane by selecting `Import` -> `Feature Class(es)` to geoprocess it.

-Add the World Imagery basemap and make the map projection "The World from Space".

-Edit the symbology of the point features and create a field within the attribute table called “month”. Change the new field’s data type to short.
  
-Open the field calculator and select “points” as the input table, month as the Field Name, and Arcade as the Expression Type. Use `ISOMonth` which returns the month of the given data from 1-12 where January is 1 and December is 12.
    
-Type ```ISOMonth($feature.timestamp)``` and select apply. The “month” field will be populated and synced with the “timestamp” field in the attribute table.

-Duplicate the points layer 11 times to have a total of 12 points for each month.
Filter each point by opening the `layer properties` -> `Definition Query` and create the query: `WHERE` “`month`” `IS EQUAL TO` #(from 1-12 in order). Each layer will correspond to a month.

-Add additional map data for the land surface to provide a visual of the habitats of the regions these birds migrate to by selecting `Map` -> `Add data` and searching for and opening the “USA Land Surface Forms” raster layer. 

Below is the final product showing all locations and habitats that the American Bitterns migrated to.

![NewLayout1](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/d624fdaa-48f5-48b0-ac57-128555086a78)


[Return to top](#jump-to-section)

## Recruiting Patterns
### 2023-24 Men's College Basketball Recruiting Footprint

In this exercise, I used data from [ESPN](https://www.espn.com/mens-college-basketball/rankings/_/week/1/year/2024/seasontype/2) and Microsoft Excel to create a map showcasing the recruiting patterns for the 2023-24 Men's College Basketball season on four of the top college basketball teams in the United States based on the preseason AP/Coaches Poll rankings.


-First, manually create a spreadsheet with player and team information obtained from ESPN. 

-Add a longitude and latitude field in the spreadsheet for each school location, find the coordinates of each school’s stadium, and copy and paste it there. Save the spreadsheet.

-Open ArcGIS Pro and go to `Analysis` -> `Tools` and search for `Geocode Addresses (Geocode Tool)`.

- Within this tool, Select the new Excel file as the Input Table, and from the drop-down menu under Input Address Locator select ArcGIS World Geocoding Service.
  
-Match the appropriate Field Name with the appropriate Alias Name by selecting the corresponding items from the drop-down menus. Click Run.

-A new layer will be created that marks all the locations of the players. Change its symbology to Unique Values and in Field 1 select School and it will automatically select different colors for each school associated with each player.

-Go to `Analysis` -> `Tools` and search for the XY to Line tool.

-Within this tool, Select the new layer as the Input Table, and from the drop-down menu under Start X Field and Start Y Field, select the hometown origin longitude and latitude respectively. Under End X Field and End Y Field, select the school longitude and latitude respectively. Check the “Preserve Attributes” box and click Run. This will create a layer with origin-destination lines.

-Change this line layer’s symbology to Unique Values and in Field 1 select School and it will automatically select different colors for each school associated with each line.
Distances

-To compare the relative distances of each school and c   I added a distance attribute to the origin-destination line layer.

-Go to `Analysis` -> `Tools` and search for the `Calculate Geometry Attributes` tool.

-Within this tool, Select the origin-destination line layer as Input Features and under Geometry Attributes, add “Distance” for Field and “Length (geodesic)” for Property. Select “Statute Miles” for Length Unit. Click Run.

-The Distance (in miles) will now be in the attribute table for the origin-destination line layer. To get aggregated statistics for this right-click it and select Statistics. This creates a Histogram chart.
Drive Time

-To compare how many players are an hour’s drive away from their school stadium, make use of the Excel file with the Long. And Lat. of each school’s stadium.

- Go to `Analysis` -> `Tools` and search for the `XY To Point tool`.

-Within this tool, select the Excel file as the Input Table and use Long. For X Field and Lat. for Y Field. Click Run.

-This creates a layer with each school’s stadium locations. 

-Now, to calculate drive-time, go to `Analysis` -> `Tools` and search for the `Make Service Area Analysis Layer` tool.

-Within this tool, create the layer name, select “Driving Time” as Travel Mode, and select “Away from facilities” as Travel Destination. For Cutoffs input 60. Click Run.

-This creates a new service area layer and tab. Select the tab and within this tool it allows me to add locations. Select the layer containing each school’s stadium locations under Input Locations. Click Apply and then click Run.

-This creates a new layer showing areas within an hour’s drive of each school’s stadium. Edit the symbology to make it visually appealing.


Below are images of the final product and statistical aggregates.

![Layout](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/e5345d11-1822-4433-86bc-47e1c212ff04)


![statsgraphic](https://github.com/KrisLaw98/KLawrence.github.io/assets/154273610/ef2ee4c0-c2e0-4758-9b3e-665da7b7d9b2)

[Return to top](#jump-to-section)

