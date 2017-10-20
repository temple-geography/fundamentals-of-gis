##Lab 6:
##Vector Operations 

#Purpose of the lab: To introduce vector operations, including buffer, map dissolve, and overlay operations. 
Data 

Note: The tutorial portion of this lab uses a variety of data layers to demonstrate how various vector operations work. After each section, remove all layers or close ArcMap. Also, the assignment portion of the lab uses a different set of layers. 

#Downloading the Data for the Lab Exercise
We will work with data made publically available by the Pennsylvania Geospatial Clearinghouse ([PASDA](www.pasda.psu.edu)) . For more details on how to download and prepare data, refer to Lab 02. 

1. From PASDA, download the following files:

- Philadelphia Bike Racks
- Philadelphia Buildings2017
- Philadelphia Planning - Neighborhoods
- Philadelphia Planning - Schools
- Philadelphia Streets - Bike Networks
- Philadelphia Streets - Zipcodes Poly
- 2015 Cartographic Boundary File, State-County-Census Tract for Pennsylvania, 1:500,000 (Note: the filename of the zipped file that you will actually download is cb_2015_42_tract_500k.zip).

Note: Some of these files you will use for the tutorial portion of the lab; others, for the assignment. They will be referred to in the rest of the document without the prefix. For instance, "Philadelphia Bike Racks" will be referred to below as "Bike Network".


Next, set up your Environment in ArcCatalog and ArcMap. This will specify a location for all of your outputs when running different operations. 

1. Select the Geoprocessing file menu, and choose Environments. 
2. Set the Current and Scratch Workspace to your Workspace, as shown below, and click OK. You do not need to modify any of the other fields. Each time you run an operation, the output will be saved to your workspace. Be sure to set up your Environment in ArcMap and/or ArcCatalog each time you begin working. 
3. In ArcCatalog, examine the properties of the shapefiles you just downloaded, paying attention.to the XY Coordinate System tab. Because these data come from the City of Philadelphia by way of PASDA, they will most like likely be in State Plane coordinates (NAD_1983_StatePlane_Pennsylvania_South_FIPS_3702_Feet). Make sure that this is the case for all of the file. Revisit earlier labs if you need a refresher or step-by-step instructions on managing coordinate systems in ArcGIS. 
4. While in ArcCatalog, you should also rename these files. For instance, The census file, you might just call PA_Tracts (as it is referred to below). The prefixes (e.g., Philadelphia Streets) aren't necessary and will only cause you grief. So you can eliminate them. As always, it always pays to make sure you can recognize the contents of a file from its name (however you go about that).  
5. Open a new ArcMap window and add the Bike Network shapefile. Remember, the data frame in ArcGIS will adopt the coordinate system of the first layer added. Open the attribute table and find the field SHAPE_Leng, which provides a measurement of the length of each individual line segment measured in US feet (the native linear unit of the State Plane coordinate system).

##Tutorial 
Note: “Vector operations” refers to a variety of tools that enable assessing interrelationships between multiple layers. While far from exhaustive, this lab tutorial walks through a set of extremely commonly used operations. Each section of the tutorial is essentially a standalone mini-tutorial that introduces how a specific operation works. You will notice that each of these operations has a variety of settings and capabilities beyond what you are instructed to do. Feel encouraged to explore the functionality of these tools. In the assignment, you will be asked to work with several of these tools in sequence, performing a classic “suitability analysis,” through which these tools identify optimal locations based on a set of criteria.

#Retrieve Line Length and Polygon Area Measurements 

Here, you will learn how to use ArcMap to calculate line length and polygon area and encode these values in an attribute table. 

Here, we will add a new field and populate the field with new length values in meters. 
1. Add a new field to the Bike Network shapefile and call it Length_m. Make it a float, and press OK. 
2. Right click on the header for the new cell (where it says SHAPE_Leng) of the field at the top of the column and go to Calculate Geometry. 
 
3. Click Yes to confirm you want to edit this field. In the Calculate Geometry dialog box make sure the Units: reads Meters [m]. Click OK. 
You should see the new length values appear in the new field. 
4. Add the Zipcodes Poly shapefile to ArcMap and open its attribute table. 
Find the SHAPE_Area field. This field encodes the area of each zip code in square feet. 
5. Create a new field called Area_m and calculate its value to be the area in square meters. Follow the same procedures as for the Bike Network but choose to calculate Area in square meters. 
6.Remove all data layers from ArcMap. 

#Buffer 

Here, you will learn how to use ArcMap to perform a buffer operation on points and lines. 
1. Add the Schools shapefile to ArcMap. 
2. The Buffer tool can be accessed two ways. For quick access, from the Geoprocessing menu, select Buffer. 
The Buffer tool can also be accessed from the ArcToolbox. Open ArcToolbox in ArcMap, and go to Analysis Tools>Proximity>Buffer. 
3. Drag Schools.shp into the input features box. Call the output shapefile schools_buf5k and make sure it saves to your workspace (which might or might not be the default location). For the Distance box, under Linear Unit, enter 5000 meters. For Dissolve Type choose ALL. Leave the rest of the options as the defaults. Press OK. 
4. View the new schools_buf5k shapefile in ArcMap. Zoom in to where there are clusters of schools to see how the buffers merge together (i.e. are dissolved as single polygons in these locations). 
5. Repeat the buffer operation with this shapefile (Schools), experimenting with different settings in the Buffer tool. For example, change the distance in the Linear Unit option to a larger value and try the NONE dissolve option. 
Note: that the operation can take a lot of time given certain settings. These types of operations can require significant processing power. 
6. Once you have created a buffer with the NONE dissolve option compare the results to the original (dissolve ALL) buffer. The key to understanding the difference is in comparing both the visible features but, as importantly, the difference in the attribute tables. 
Note: in the image that in the undissolved buffer there is a one-to-one correspondence between each original object and the buffers the operation creates. The attribute table looks precisely like the attribute table from the Schools file. In the dissolved buffer, however, the result combines all of the buffers into a single object. You can think of this as a binary: the areas within 6 km of an educational institution (inside the shapefile) as opposed to those that are not. We will used dissolved buffers later on this lab because they are useful analytically (as you’ll see).

6. Open the attribute table for schools_buf5k, noting again that although there are multiple polygons displayed on screen, there is only a single record in the attribute table (this is called a “multipart object”). 
7. In some cases, it is useful to have an individual record for each feature. To make this conversion, where each polygon is represented as a single record in the attribute table, go to ArcToolbox?Data Management Tools>Features>Multipart to Singlepart and input schools_buf5k and name the output schools_buf5k_single. Open the output table from the resulting shapefile and confirm that it has multiple records — one for each feature. 

7. When you are finished, close ArcMap. 

#Map Dissolve 

Here, you will learn how to use ArcMap to generalize a data layer by dissolving all of the features and dissolving into adjacent polygons with identical values. 
1. Add the PA_Tracts shapefile. Go to the Geoprocessing file menu and select Dissolve. Note that the Dissolve tool can also be accessed from ArcToolbox (ArcToolbox>Data Management Tools>Generalization>Dissolve). 
2. Drag the PA_Tracts shapefile into the input box. Name the resulting file PA_outline. Leave all the other commands alone and press OK. Take a close look at the attribute file and the result. The operation has dissolved all the boundaries between the tracts, resulting in an outline of the entire set of features (i.e., the entire state of Pennsylvania).  You’ll notice that the resulting feature has one entry in the attribute table.

3. Now compare what you have just done to what happens when you dissolve by an attribute. Open the attribute table of PA_Tracts and find the field COUNTYFP. This field is a 3-digit identifier for each individual  county (e.g. 091 for Montgomery County) in the state. You'll notice that the GEOID code full name for all the tracts in Philadelphia begins 42101 (42 for Pennsylvania; 101 for Philadelphia County). Of course, many individual tracts share the same value for this field. It is thus possible to perform a map dissolve operation on the tracts_utm shapefile, using the FIPSSTCO field as the dissolve field. 
4. Drag the tracts_utm shapefile into the input box. Name the output shapefile PA_Counties. Under the Dissolve Field option click COUNTYFP. Press OK. 
5. View the resulting shapefile in ArcMap. You'll see that the tracts have been dissolved by the county identifier, resulting in a shapefile of county outlines. 
6. Remove all layers from ArcMap. 

#Point in Polygon Overlay 

Here, you will learn how to use ArcMap to perform a point in polygon overlay, also called a “spatial join”.  You have seen such an operation before. When you need to know which polygon a point is within, you can do so by joining the attributes of a polygons layer to that set of point. Here, we will determine which zip code each of the point locations from the Schools shapefile lies within. 
1. Add the Schools shapefile point shapefile and the polygon Zipcodes_Poly shapefile to a new ArcMap window. Review each of their attribute tables and note the fields that are included in each. 
2. Right click the Schools shapefile and choose Joins and Relates>Join. In the new window, the top drop down menu should read “Join data from another based on spatial location”. 
3. Choose Zipcodes_Poly as the layer to join to the Schools layer. You are given a choice of how to associate each polygon with a given point. Here, choose “it falls inside”. 
4. Name for your new shapefile (Schools_wZip will suffice). 
5. Press OK
6. View the new shapefile's attribute table. You will observe that the attributes from the Zipcodes_Poly file are now appended to the attributes of the original Schools shapefile. 

You can also join points to polygons. The problem, however, is that many points can fall within a single polygon. Since we cannot store multiple values of a single attribute for a single polygon (a violation of first normal form), the software provides a few options for transforming potentially multiple point values to a single value for each polygon. 
1. Right click on the Zipcodes_Poly shapefile and choose Joins and Relates>Join. In the new window, the top drop down menu should read “Join data from another based on spatial location”. 
2. Choose the Schools shapefile as the layer to join to the Zipcodes_Poly layer
3. Note that it now reads “You are joining: points to polygons”. You are given a choice for how to summarize the point data. Option one counts the number of points within each polygon and then offers some statistical functions based on combining the attributes of multiple points that fall within each polygon. Option two simply takes the point nearest the polygon, or if many points fall within a single polygon, it takes the first point that the algorithm comes to. 
4. Here, choose the first option (you don’t have to choose any statistical summary option). 
5. Choose a name for your new shapefile. 
6. View the new shapefile and open its attribute table. You should see a new field called Count that contains the number of Schools points that fall within each polygon. 
7. Remove all the layers from ArcMap. 

#Line in Polygon Overlay 

Here, you will learn how to use ArcMap to perform a line in polygon overlay. 
You can also do a line in polygon overlay in a manner analogous to the point in polygon overlay demonstrated above. Here, however, multiple lines can fall within a single polygon, and multiple polygons can fall across a single line. Thus, the user must supply the method of summarizing multiple values for both spatial joins from polygon onto line layers and from line onto polygon layers. 

1. Use the Bike_Network and Zipcodes_Poly shapefiles in ArcMap to experiment with different line in polygon overlay parameterizations. Try the spatial join in each direction as you did with Schools and Zipcodes above. 
2. Observe the shapes themselves as well as the attribute tables and note how new attributes have been appended as a result of the operations.
3. Remove all the layers from ArcMap. 

#Polygon Overlay: Clip 

Here, you will learn how to use ArcMap to perform a clip operation. 
A Clip operation is akin to a “cookie cutter” operation whereby one layer is clipped to the boundaries of another. 
For instance, say you were interested in which buildings fall within the City of Philadelphia's recognized neighborhoods, it is possible to extract the buildings that fall within these neighborhoods as a separate shapefile.
1. Add the Neighbhorhoods and Buildings shapefiles to ArcMap. 
2. Create a new shapefile that represents only the neighborhoods around Temple's Main Campus Montgomery County. You can do this by using a Select by Attributes operation, selcting the following neighborhoods: NORTH CENTRAL, CECIL B. MOORE, YORKTOWN, and NORTH PHILA. Export the selected features to a new shapefile called TU_area. Make sure the file exports to your workspace (again, this location might not be the default) and is specified to export as a shapefile. 
3. Go to the Geoprocessing file menu and select Clip. Note that the clip tool can also be accessed from ArcToolbox (ArcToolbox>Analysis Tools>Extract>Clip). 
4. Drag Buildings into the input features box. Drag TU_area into the clip features box. Name the output shapefile Bldgs_TU. 
5. View the new shapefile. It should include only those building footprints that are within the bounds of the TU_area shapefile. 
6. Remove all the layers from ArcMap. 

#Polygon Overlay: Intersection and Union 

Here, you will learn how to use ArcMap to perform an two polygon overlay operation--Intersection and Union. 
The intersection overlay operatoin creates a new data layer from the areas that overlap when you overlay multiple polygons shape files (that is, the areas that are spatially coincident). Say you were interested in finding which zipcodes of the areas within 5 km of a school. An intersection operation would show you what these are.  

1. Open ArcMap add Zipcodes and the schools_buf5k and that you created earlier. 
2. Go to the Geoprocessing file menu and select Intersect. Note that this tool can also be accessed from ArcToolbox (ArcToolbox>Analysis Tools>Overlay>Intersect). 
3. Drag schools_buf5k and Zipcodes into the input features box. Call the output zip_edu_bufint
4. View the resulting shapefile. It should include the area spatially coincident to both layers. Now view the attribute table. It should also contain attributes of both input shapefiles. Note that the Area_M file is no longer up to date and would need to be recalculated (if used for subsequent analysis). In geodatabases, area and length fields are updated automatically. 

Now we'll perform a similar overlay, this time using the Union tool (see the Geoprocessing file menu). 
1. Choose the schools_buf5k and Zipcodes shapefiles again and call the output zip_edu_bufun.shp. 
2. View the result. The resulting shapefile should contain those areas that are within 5 km of a school or are in a zip code (i.e. all the area covered by both polygon data layers). 
3. The key to understanding the output of the union is the attribute table. In the attribute table, find the field that begins FID_. Note that there are different values for this field, including -1. The values that are greater than -1 indicate that this polygon is inside the educ_buf5k buffer, while a value of -1 indicates the polygon is outside the buffer. This is how ArcGIS keeps track of what is inside or outside of a buffer. To demonstrate this, perform a selection selecting those polygons that are greater than 1 for this field. You should see polygons within the education shapefile buffer selected. Switch selection to see the polygons outside the educational shapefile buffer selected. 

##ASSIGNMENT 
#Objectives 

Biking has become an increasingly popular mode of transportation in Philadelphia, and bike advocacy organizations have started to lobby for the City to install additional bike racks. A number of bike racks have been installed along the sidewalks with heavy bike traffic. Although the number of bike racks around the Temple area has increased substantially over the past few years, so has the number of people looking to lock their bikes near campus, forcing a lot of people to improvise. 

The objective of this assignment is to identify locations in the neighborhoods surround Temple University's Main Campus (i.e, the shapefile that you created in the tutorial on the using the Clip function). 
Note: the City of Philadelphia's bike racks are separate from Temple University's. Assume for the purpose of the lab that the city is using this analysis as a first cut to determine suitable areas. A second step, not part of this lab, would then be to determine which of these areas are already served by Temple University bike racks.

These bike racks should be: 

1. Within 100 meters of the Philadelphia bike network 
2. Within 30 feet of a building 
3. Not within 200 feet of an existing bike rack.
4. Within a mimimum area of 50 square meters.

#Deliverables 

Turn in a report addressing this objective. This report should include a map of the potential area(s) in which to add a bike rack. All shapefiles should be in StatePlane Pennsylvania South. Your lab should also include a flow chart/cartographic model to illustrate your methodology. 

#Getting Started 

You should use the following data files that you have already downloaded from PASDA and prepared: 
* Bike Racks
* Bike Network
* Buildings
* Neighborhoods 

When you perform spatial operations, the measurements are calculated in terms of the measurement units of the assigned coordinate reference system. Therefore, before you begin working, make sure all layers are in State Plane Pennsylvania South coordinates. 

#A few suggestions:

1. Buffer the Bike Racks, Bike Networks, and Buildings shapefiles based on the distance criteria listed abovey using the Buffer tool (Analysis Tools>Proximity>Buffer) to calculate the buffers around the bike network, buildings, and existing bike racks. 
2. When buffering, make sure to choose Dissolve Type = ALL. Additionally, when you are buffering the buildings, please choose the Side Type to be OUTSIDE_ONLY so that your buffer does not include the building footprint. 
3. Once you have your buffers, clip the buffered features to the neighborhoods adjacent to Temple using the TU_area shapefile created above. 
4. Using a combination of the intersect and union tools select areas that are within 75 feet of a bike network and 20 feet of a building but NOT within 200 feet of an existing bike rack. 
5. Once you have selected the suitable bike rack areas, you may need to use the Multipart to Single Part tool to create a new record for each polygon. You will then need to calculate the area for each of the polygons. 
NOTE: All of the tools that you need to complete the lab are described in the lab tutorial. If you are unsure of where to find a tool or how to use it, please refer back to the description in the tutorial or ask the instructor. 

#Requirements 

1. Analysis: Report describes the correct approach. 
2. Writing: Report thoroughly addresses all sections, employs appropriate technical language, and is free of grammatical mistakes. 
3. Figures: Report includes one map that correctly displays the requested information and one flow chart that documents each of the steps followed. 
 

