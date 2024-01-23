# GIS Portfolio
Welcome to my GIS portfolio. Below you can browse examples of my work compiled over the past two years. 

This portfolio can also be viewed by visiting [https://). 

## About me 
<img align="left" src="https://user-images.githubusercontent.com/32546509/77238966-de37cd00-6bab-11ea-9b9b-e49783f8300a.jpg">
My name is Kristoff Lawrence and I’m a recent graduate of The University of Technology, Jamaica's Urban and Regional Planning Bachelor's program (B.Sc. URP). Before enrolling at UTech, I worked for a few months at the Island Traffic Authority's May Pen Depot, where I was an office assistant.<br />
<br />
I first became interested in Geographic Information Systems during my latter years at UTech since it was part of my courses. I had no prior knowledge of it but was interested in how I could manipulate this software to solve or highlight problems.<br />
<br />
This realization is what ultimately led me to enroll in Michigan State University's Professional GIS Certificate program. As one of the country’s top-rated GIS certificate programs with a mission of training students to “turn raw data into solutions for society’s most pressing problems,” the MSPPM program at Carnegie Mellon offered the perfect opportunity to develop the technical competencies I was seeking as well as an ideal environment to practice those skills in a real-world setting—namely, the City of Pittsburgh and its diverse network of organizational partners.<br />
<br />
My most recent projects have focused on public interest technologies that help to increase the range and accessibility of social services worldwide, especially as a means of increasing society's resilience to global threats like climate change.<br />
<br />
View my resume [here](https://jaxgoodlabs.github.io/patrick-campbell-portfolio/Patrick%20Campbell%20Resume.pdf).

#### Jump to Section
- [Basic Network Visualization and Routing](#basic-network-visualization-and-routing) 
- [Creating a Landuse Map ](#creating-a-landuse-map ) 
- [Interpolating Point Data](#interpolating-point-data)
- [Locating Nearest Facility with Origin-Destination Matrix](#locating-nearest-facility-with-origin--destination-matrix) 
- [Creating a Map From an Online Mass Shooting Database](#creating-a-map-from-an-online-mass-shooting-database) 
- [World Earthquakes](#world-earthquakes) 
- [Service Area Analysis using Openrouteservice](#service-area-analysis-using-openrouteservice)
- [Creating an Airport Location Map Using Python Scripting](#creating-an-airport-location-map-using-python-scripting) 
- [DEM of Crystal Mountain National Park, Gabon](#dem-of-crystal-mountain-national-park,-gabon) 
- [Norwalk Transportation Project](#norwalk-transportation-project)
- [Pittsburgh EV Site Analysis Project](#pittsburgh-electric-vehicle-site-analysis) 
- [Optimizing Coverage of Hunting Areas for Maximum Removal of Burmese Pythons in South Florida](#optimizing-coverage-of-hunting-areas-for-maximum-removal-of-burmese-pythons-in-south-florida) 
- [Bumblebee Conservation in the Midwest US](#bumblebee-conservation-in-the-midwest-us)
- [Resilience Oriented Planning Map](#resilience-oriented-planning-map)

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

-First I access the data from the Cape Town Open Data Portal using the ArcGIS REST server URL to create a server connection, downloading three layers: Cape Town CBD layer, Zoning layer, and Split Zoning layer.

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

-First I obtain the shapefiles for Lake Arlington's 2007-12 survey from the TWDB website. 

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

## World Earthquakes ( Magnitude 5 - 8 )

### Earthquake Hazard Frequency and Distribution ( August 27 - October 27, 2023)

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

<p align="center">
<img width="100%" height="100%" src="https://user-images.githubusercontent.com/32546509/83973007-78590880-a8b1-11ea-8c1f-6ab0df87929f.png">
</p>

Click [here](https://jaxgoodlabs.github.io/SustainableCity/) to view the interactive final product.

[Return to top](#jump-to-section)

## Norwalk Transportation Project

<p align="center">
<img width="100%" height="100%" src="https://user-images.githubusercontent.com/32546509/83985569-c00a7f00-a907-11ea-801d-83699e1ea831.png">
</p>

Click [here](https://github.com/jaxgoodlabs/Norwalk_Transportation/) to view this project on GitHub.

[Return to top](#jump-to-section)

## Pittsburgh Electric Vehicle Site Analysis

<p align="center">
<img width="100%" height="100%" src="https://user-images.githubusercontent.com/32546509/80050002-79410100-84e2-11ea-9ced-5082f60267e9.JPG">
</p>

Click [here](https://github.com/jaxgoodlabs/Pitt_EV_Bike_Ped_Site_Analysis) to view this project on GitHub.

[Return to top](#jump-to-section)

## Optimizing Coverage of Hunting Areas for Maximum Removal of Burmese Pythons in South Florida

<p align="center">
<img width="100%" height="100%" src="https://user-images.githubusercontent.com/32546509/80058259-cb8c1d00-84f6-11ea-919a-03ca89bd5605.JPG">
</p>

Click [here](https://jaxgoodlabs.github.io/sfwmd-python-program) to view the final report for this project.

[Return to top](#jump-to-section)

## Bumblebee Conservation in the Midwest US

<p align="center">
<img width="100%" height="100%" src="https://user-images.githubusercontent.com/32546509/83971918-9ae82300-a8ab-11ea-82e8-bf18962a0587.JPG">
</p>

[View this map on ArcGIS Online](https://arcg.is/05XH5S0) or click [here](https://carnegiemellon.maps.arcgis.com/apps/MapSeries/index.html?appid=9dc0bb2905164cf4a9c71c6e6a972c63) to view the project Story Map. Access the final report [here](jaxgoodlabs.github.io/bumbleebee-conservation/).

[Return to top](#jump-to-section)

## Resilience Oriented Planning Map

<p align="center">
<img width="100%" height="100%" src="https://user-images.githubusercontent.com/32546509/83976614-14dad500-a8c9-11ea-829d-abff9be35343.png">
</p>

Click [here](https://jaxgoodlabs.github.io/pittsburgh-hazards-map) to view this project.

[Return to top](#jump-to-section)
