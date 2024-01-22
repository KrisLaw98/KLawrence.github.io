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



In this exercise, I used data from the [The Violence Project Mass Shooter Database](https://www.theviolenceproject.org/mass-shooter-database/) and [Homeland Infrastructure Foundation-Level Data (HIFLD) Open Data](https://hifld-geoplatform.opendata.arcgis.com/datasets/geoplatform::local-law-enforcement-locations/explore?filters=eyJUWVBFIjpbIkxPQ0FMIFBPTElDRSBERVBBUlRNRU5UIl19&location=26.839486%2C-96.938262%2C3.00) to create map showcasing mass shootings within the United States of America from August 1, 1966 to May 6, 2023. 

-First, I downloaded the first Excel spreadsheet with the mass shooting data in it from the database. I then Converted it to a .csv format and imported it to QGIS as a Delimited Text Layer. 

-Changed the symbology to heatmap and weigh points by number killed, changed blending mode to multiply, and changed radius to 8 mm. Changed the Label to Single Label and Value to Number Killed and rendered it to a scale of 1:4725400  so it shows when zoomed at that scale.

-Filtered the data from the Local Law Enforcement Locations feature class/shapefile and specifically downloaded the Local police department Locations from Homeland Infrastructure Foundation-Level Data (HIFLD) as an Excel spreadsheet. Convert to a .csv format and import into QGIS as a Delimited Text Layer but with a point coordinate geometry definition and change symbol blending mode to multiply and size to .4 mm.

-I also downloaded the Excel spreadsheet with mass shooting community-level data that contains demographic information. Convert to .csv format and then import to QGIS as Delimited Text Layer. Filter this new layer for 1 or more gun stores in zip code -> "N Gun Stores in Zip Code" >= 1.

-Execute the Join Attributes by Field Value algorithm. The first Input Layer is the regular mass shooting data with table field as total firearms brought to the scene, second input is MS Community data with the Table Field data as N of gun stores in zip code. Select discard records which can’t be joined.

5.	In the new joined layer change the symbology to Graduated with N Gun Stores in Zip Code as the Value. The mode is Fixed Interval with size 1 and with Classes set to 4.

Display

The heatmap I used for the symbology in Step #1 does not display in the legend for the print layout. I researched a way to do this and I found out about the Heatmap (Kernel Density Estimation) tool. I used the MS Database layer as the point layer and left everything to default and then I ran it. A new heatmap raster was created and then I edited the symbology – I set the render type to singleband pseudocolor with the Min and Max set to the lowest no. of people killed and most no. of people killed respectively. I set the mode to equal interval and put a minimum of 19 classes to create a makeshift heatmap for the legend in theprint layout.



Images of the final product are displayed below.

[Return to top](#jump-to-section)

## World Earthquakes

While exploring my location data in ArcGIS data - which curiously included data just from 2015, 2019 and 2020 - the first thing that struck me was how spotty the coverage was. I had thoroughly expected to see a complete saturation of points throughout my travel range, but for many trips, found only single points representing a final destination. These were long, drawn out car trips, some of them covering multiple days of travel, and yet Google seems to have only collected a single data point from the whole trip.

The two notable exceptions to this rule are my more regular commutes, e.g., between Miami and West Palm Beach when I was living in West Palm Beach and hunting pythons in Miami on the weekends, and longer trips that I took more than once, e.g., my trip from my permanent residence in Jacksonville, FL up to school in Pittsburgh. A lot of my traveling, particularly in Florida. passes through cell-coverage dead zones, so I assume that of the spottiness in data collection owes to that fact. That might also explain the single erroneous point near Fort Walton Beach in the Florida panhandle. While I passed in that general vicinity on one or two occasions during this period, that particular point is pretty far off from any of the routes I would've taken. 

My dead zone theory is also consistent with the absence of points west of the Tamiami Trail/Krome Ave. intersection in Sweetwater, FL. This area, which passes through Everglades and Big Cypress National Parks, is where I did all my hunting for the python program, but has notoriously poor cell coverage. 

The other interesting thing I noticed about my location data is how sprawling my data is around the areas that I live. The first thing I do when I settle into a new place is starting exploring the nearest natural areas with hiking opportunities. Often, the closest locations are maybe a half-hour drive from my apartment in the city, but after a year or two, I'm up to driving two or three hours away to find new parks to explore. This pattern is most noticeable around Pittsburgh (my current residence) and Jacksonville, FL (my permanent residence). 

<p align="left">
<img width="38.1%" height="38.1%" src="https://user-images.githubusercontent.com/32546509/79076040-e8d61580-7cc4-11ea-8a5f-941fb5e093a2.JPG"> <img width="60.3%" height="60.3%" src="https://user-images.githubusercontent.com/32546509/79076036-e2e03480-7cc4-11ea-9b77-d68aa0b6e1d1.JPG">
</p>

I was surprised at how little order I could discern at a high scale. If I had no special knowledge, I doubt I would be able to discern from the concentration of points alone anything beyond where I lived and where I've been. My work and school locations were not at all obvious even at the narrowest zoom ranges (although perhaps referencing what's located at each of these points in conjunction with the date and time stamps would clue me in).

The location of my home was easier to discern, but still required some fine-combing through the data. Of course, there's a lot more information embedded in this data than is apparent in the distribution of the points alone, and I imagine that it wouldn't be difficult to answer these sorts of questions once my data was aggregated with that of other users, since these patterns probably don't differ too drastically between members of the same general category (e.g., university students). 

To see how easily I could extract this sort of information from these data, I used the hexagon binning option in the aggregate points tool to aggregate the number of points located in each tenth mile squared cell. Sure enough, this method resolved all the ambiguity of the previous display. Using a contrasting blue-yellow color scheme made my home and frequent locations pretty unmistakable, as the image below demonstrates.

<p align="center">
<img width="100%" height="100%" src="https://user-images.githubusercontent.com/32546509/79072713-7a875800-7cb0-11ea-9c13-60147f423540.JPG">
</p>

[Return to top](#jump-to-section)

## Service Area Analysis using Openrouteservice

<p align="center">
<img width="100%" height="100%" src="https://user-images.githubusercontent.com/32546509/80930537-2a1c8b00-8d82-11ea-9803-c63422e9555e.JPG">
</p>

*Overview*

This project aims to model the flood risks for a single census tract in Miami, FL. This census tract  is currently being analyzed as part of a pilot study by researchers at Miami-Dade College. The methodology developed over the course of this study will eventually be adapted for application to the entire city in order to better anticipate and respond to the threats posed by climate change to public buildings and the communities that they service.

(Updates to the proposed workflow and deliverables are provided at the end of this summary.)

**1.	Problem Statement**

The problem this project aims to solve is how to best leverage FEMA’s Hazus tool for risk assessment, policymaking, and coordinated response in Miami, Florida, especially in the face of rapidly changing climate conditions and rising seas.

A secondary challenge related to the problem defined above is how to design / optimize the workflow for city-wide deployment of the developed methodology. This requires, among other things:
-	Determining the optimal resolution of drone footage to balance (a) speed and efficiency of data collection and processing with (b) accuracy and reliability of assessments and recommendations
-	Integrating multiple software platforms (CDMS, ArcGIS Pro + Hazus, etc.) and data types (high-resolution aerial imagery, structural data, GIS map layers, etc.).

**2.	End Users**

The end users of the products generated by this project are researchers at Miami-Dade College and the City of Miami.

**3.	Work Plan and Methodology**

This project aims to model the flood risks for a single census tract in Miami, FL. This census tract  is currently being analyzed as part of a pilot study by researchers at Miami-Dade College for eventual expansion to the entire City of Miami. 

The two central elements of the project are (1) the flood model (Hazus Flood Level 1) for the target study area and (2) a documented workflow detailing how the data collection, processing, and cross-platform integration will be accomplished.

Key steps in this workflow include:
-	Collection of high-resolution aerial imagery of target study area using drones
-	Processing and analysis of drone footage to model key features of site, including buildings and infrastructure for more accurate and precise damage projections
-	Integration of drone data into Hazus state data package using Hazus Comprehensive Data Management System (CDMS) and other software systems as necessary
-	Codification of site analysis within Hazus tool
-	Export of results and findings for response and policymaking

My methodology for this project varies according to the different component activities it comprises. For a thorough description of methods employed organized by activity, refer to the accompanying [Process Log](). 

**4.	Group Members and Roles**

Patrick Campbell - Project Manager, GIS Analyst, Data Systems Manager

**5.	Data Sources**

The full set of source data used for this project is detailed in the accompanying [Data Documentation]() file. 

**6.	Final Deliverables**

The final deliverables for this project include:
-	A one-page summary describing what my final project is, who worked on it, what the problem I’m attempting to solve is, and who benefits from it. 
- A [Process Log]() documenting the steps involved in my analysis, organized by date. 
-	Final assets: 
  - Final project package including 
    - Hazus Flood Modeling for Miami, FL geodatabase file
    - Screenshots
    - Source documents
    - Esri Surface Imperviousness Exercise (including all associated outputs)
    - MiamiStudyArea mxd file for Hazard Analysis
  -	[Example tutorial]() for creating a flood model and impact assessment from a Digital Elevation Model (DEM) file

**7.	Project Updates**

<p align="center">
<img width="85%" height="85%" src="https://user-images.githubusercontent.com/32546509/80931010-4d950500-8d85-11ea-9002-b4b24cb4d7a9.png">
</p>

Due to difficulties running the Hazus software on my personal laptop, I decided instead to focus on analyses I could execute using the standard ArcGIS Pro suite of tools. The central focus of this effort involved attempting to reproduce several official flood models issued by Miami-Dade County and NOAA. For example, in the figure below, I've reproduced [NOAA's model for 9 feet of sea level rise](https://coast.noaa.gov/slr/#/layer/slr/9/-8926859.280505564/2977300.725765961/15/satellite/none/0.8/2050/interHigh/midAccretion) by converting a Digital Elevation Model (DEM) file to a polygon layer representing flooded lands. Details of this and other analyses are thoroughly documented in the accompanying [Process Log]().

<p align="center">
<img width="100%" height="100%" src="https://user-images.githubusercontent.com/32546509/80931424-c09f7b00-8d87-11ea-8a65-91b0f92a0d36.JPG">
</p>

A secondary goal of this project was to troubleshoot various tutorials and flood modeling exercises for possible incorporation into the new Geographic Information Systems for Environmental Awareness and Community Engagement (GISEC) curriculum at Miami-Dade College. “The goal of GISEC is to create a pathway for students in GIS technology and to enhance technological applications of public interest with an emphasis on environmental hazards awareness and community engagement in Miami-Dade County, a minority-majority urban area significantly impacted by, and at ongoing risk for, natural disasters.”[1](https://www.mdc.edu/entec/grants/gisec.aspx)  To this end, I spent a large portion of my time on this project exploring various applications of the ArcGIS Pro Image Classification tools, completing several guided tutorials in the process. The products of these exercises have been included in the final project package. Relevant notes and insights from these exercises are provided in the Process Log, while the Example Tutorial file provides a model of how these analyses might be converted into guided tutorials for use in the classroom. 

[Return to top](#jump-to-section)

## Creating an Airport Location Map Using Python Scripting

<p align="center">
<img width="100%" height="100%" src="https://user-images.githubusercontent.com/32546509/80049872-1e0f0e80-84e2-11ea-99b5-3ff09698fc8c.JPG">
</p>

Click [here](https://jaxgoodlabs.github.io/GIS_portfolio/sustainable-campus) to view the final report for this project. The interactive final product can be accessed [here](https://jaxgoodlabs.github.io/Sustainable_Campus/).

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
