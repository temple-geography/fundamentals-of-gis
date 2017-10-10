---
title: Creating Geographic Data Through Georeferencing and Digitization
author: Hamil Pearsall
---

**Purpose:** The purpose of this exercise is to introduce the basics of georeferencing and digitizing in ArcGIS. 

# Data

The data for this tutorial includes an historical map image hosted on the course GitHub repo, and two modern GIS layers which you will download from [MassGIS](https://www.mass.gov/service-details/massgis-data-layers), Massachusetts' spatial data repository. The towns and ponds layers we are interested in are in ZIP archives (linked below) which contain other layers. Download and unzip the archives, but keep in mind that we won't be using all of the data downloaded, and make sure to add the correct layer when directed to.

* [Worcester_towns.jpg](images/Worcester_towns.jpg): Right-click the image name (to the left) and save it to your workspace. This is a JPEG image of an 1871 map of towns in Worcester County, Massachusetts, downloaded from DavidRumsey.com. [Click for more information about this image.](https://www.davidrumsey.com/luna/servlet/detail/RUMSEY~8~1~26368~1100042:Worcester-County-?sort=Pub_List_No_InitialSort%2CPub_Date%2CPub_List_No%2CSeries_No&qvq=q:towns%2Bin%2Bmassachusetts;sort:Pub_List_No_InitialSort%2CPub_Date%2CPub_List_No%2CSeries_No;lc:RUMSEY~8~1&mi=29&trs=294)
* `TOWNS_POLY.shp`: A shapefile of the town boundaries in Massachusetts. It is included in the [Community Boundaries (Towns)](http://www.mass.gov/anf/research-and-tech/it-serv-and-support/application-serv/office-of-geographic-information-massgis/datalayers/towns.html) bundle.
* `MAJPONDS_POLY.shp`: A shapefile of major ponds in Massachusetts. It is included in the [Major Ponds and Major Streams](http://www.mass.gov/anf/research-and-tech/it-serv-and-support/application-serv/office-of-geographic-information-massgis/datalayers/majhd.html) bundle.

# Georeferencing

Georeferencing is the process of aligning data with real-world coordinates. In this tutorial we will geoference a historic map of towns in Massachusetts to a modern GIS layer of town boundaries, and we will lakes in the Town of Auburn and compare them to a current shapefile of lakes. This process will take a few steps.

A video of this georeferencing tutorial is available at <https://drive.google.com/file/d/0B_rk0_Y4N6QzdEg3RmFSY2tjUUU/view> and a brief description of the steps follow. You might find it useful to review the steps below and watch the video before beginning the tutorial.

1. Open (using the system image viewer, not ArcMap) and review the JPEG image of towns in Worcester, Massachusetts, 1871. Close the image.
2. Open ArcMap.
3. Add `TOWNS_POLY.shp` to the map canvas.
4. Set the polygon fill to hollow, so that you can see the town borders, but also see the historical map (which we are about to add).
4. Add the 1871 towns map to ArcMap. Even though this is not a GIS data layer, the file will show up in the Add Data dialog box as a suitable data source.
5. You will get the by now familiar Unknown Spatial Reference warning. This is OK! This is the problem that you are about to fix! The image will not appear in the map initially, but you will be able to see the name `Worcester_towns.jpg` in the TOC, so you will know that it is there.
6. Open the Layer Properties for the Worcester towns layer and select the Display tab.
7. Change the Resample during display setting to Cubic Convolution. This will improve the quality of the display.

The data are now available in your map document and you are ready to begin Georeferencing.

1. Go to the Customize→Toolbars menu. A long list of toolbars will appear.[^toolbar] Turn on (select in the menu) the Georeferencing toolbar. The Georeferencing toolbar will appear floating over your map canvas. You can drag this to a position that you like, including docking it to the top (among the other toolbars), bottom, or sides of the ArcMap window.
2. As we have only one image, Worcester\_towns.jpg, in this map document, it should already appear in the dropdown of the layer to be georeferenced. If for some reason it is not, select the layer name in the dropdown.
3. From the Georeferencing menu that appears in the toolbar, select Fit to Display. The JPEG image will now appear in the visible part of the map canvas, along with the rest of the layers you added.

[^toolbar]: You can also show the list of toolbars to display or hide by right-clicking anywhere in the toolbar area, i.e. on an visible toolbar or in the gray space next to a visible toolbar.

You are now ready to begin placing ground control points (GCPs). The GCPs link coordinates from the map (already referenced) with the image of the historical map.

1. In the Georeferencing toolbar, click the Add Control Points button ![Add Control Points](images/ArcMapAddControlPointsButton.png).
2. Place the first GCP:
    a. Look at the modern towns layer and the map to identify (by shape, or possibly by name) a town that is the same in both data sources.
    b. Click on a corner of the town in the JPEG image. ***NOTE:** You **must** click on the image **first***.
    c. Click on the matching corner of the town in the `TOWNS_POLY` layer. The image will immediately reposition itself so that that point in the two data sources are aligned. The scale (size of the towns) will (probably) still be off.
    d. Repeat the process for another (perhaps the opposite) corner of the same town. The image will now be rescaled to approximately the same size as the modern towns layer. It won't be perfect, but even with only two GCPs, it should now be visibly obvious that the two data sources are slightly different versions of the same real-world entities.
3. Place additional GCPs.  As you place additional points, the image will align more and more closely to the modern data. 
4. In addition to visually assessing the fit, you should also evaluate the fit using the residuals and **root mean square error (RMSE)**. Click the View Link Table button ![View Link Table](images/ArcMapViewLinkTableButton.png) on the Georeferencing toolbar. The residual tells you how much discrepancy lies between the reference data (modern towns layer) and the JPEG image. Values close to 0 indicate that you are doing a good job. If the RMSE reaches 0, you have perfect fit. You can delete control points that have a very large residual, and you may find that the fit will be improved by deleting these points.
5. Continue placing additional GCPs. Ideally, your control points will be well-distributed across the map. We can't know the number of required GCPs in advance. The process is iterative. As you place additional points, you will evaluate the fit, erase the ones with a poor fit, add new points to improve the fit, etc. **NOTE:** The next section of the lab will focus on digitizing features in the town of Auburn, so please prioritize the fit in Auburn.
6. When you are satisfied with your GCPs, you will want to save the JPEG as a new image. 
    a. Select Rectify from the Georeferencing menu.
    b. Change the resample type to Bilinear Interpolation.
    c. Select the folder (Output Location) where you would like to save the image. **NOTE:** The Select Workspace dialog is somewhat confusing. Do *not* try to enter a filename. Just select the folder and hit Add.
    d. Set the Format to TIFF (if necessary--it should be the default).
    e. Name the image (if you are not happy with the default name).
    f. Hit Save.

# Digitizing

For historical research, the researcher/analyst will often need to **digitize** features by referring to an historical map. Now that you have finished geoferencing the towns map, you can begin digitizing features drawn on the map. We are going to digitize the lakes in the town of Auburn and compare the historical locations of these lakes to the current water bodies taken from a modern GIS layer.

A video of this digitizing tutorial is available at <https://drive.google.com/file/d/0B_rk0_Y4N6QzWFFDclRUcnE3TkU/view> and a brief description of the steps follow. You might find it useful to review the steps below and watch the video before beginning the tutorial.

1. Add your georeferenced .tiff to the map document.
2. Open the ArcCatalog pane in ArcGIS. Right click in your folder and select New→Shapefile.
3. Create the settings for your shapefile as follows:
    a. Name: `Lakes_auburn`
    b. Feature type: polygon
    c. Spatial reference: Select the Edit button, then Import and in the following window, select the `TOWNS_POLY` shapefile. This will import the same spatial reference as the `TOWNS_POLY` shapefile.
4. Click OK and the new shapefile will be added to your workspace. If it does not get added automatically, you can use the Add Data button as usual.
5. Right click in the grey space near the file menu and select the 'Editor' tool bar.
6. Select Editor and then 'Start Editing'
7. Select your newly created 'Lakes_auburn' shapefile to begin editing.
    a. You will see a new pane appear on the right. Your new shapefile should appear. 
8. Select the shapefile, and look at the bottom of the pane. You should see a number of 'Construction' tools appear. 'Select Polygon'.
9. Begin tracing the outline of one of the lakes in the town of Auburn with a series of clicks that are not too far apart:
10. When you have finished one polygon, double-click to complete the sketch.
    a. The polygon will out outlined in teal, indicating that the shape is complete.
    b. Each time you complete a feature (e.g. polygon) return to the Editor Tool Bar, and select Save Edits from the drop down so that you do not lose any work.
11. Digitize all of the lakes in the town of Auburn and when you are finished, save your edits once again and chose 'stop editing' from the Editor dropdown.
12. Add the `MAJPONDS_POLY.shp` layer to compare the locations of lakes in 2017 and 1871.

# Assignment

In 2016 there were a number of articles about the shrinking Great Salt Lake <https://earthobservatory.nasa.gov/IOTD/view.php?id=88929> in Utah.

How much has the Great Salt Lake in Utah changed since 1889? You will start with an historical image of Utah. You will georeference the image, digitize the perimeter of the the Great Salt Lake, and compare the digitized lake to a modern data source. As with the Worcester towns historical map, [this data is downloaded from DavidRumsey.com](https://www.davidrumsey.com/luna/servlet/detail/RUMSEY~8~1~37442~1210316:Utah-?showTipAdvancedSearch=false&showShareIIIFLink=true&showTip=false&helpUrl=https%3A%2F%2Fdoc.lunaimaging.com%2Fdisplay%2FV73D%2FLUNA%2BViewer%23LUNAViewer-LUNAViewer&title=Search+Results%3A+List_No+equal+to+%272094.058%27&fullTextSearchChecked=&dateRangeSearchChecked=&advancedSearchUrl=https%3A%2F%2Fdoc.lunaimaging.com%2Fdisplay%2FV73D%2FSearching%23Searching-Searching&thumbnailViewUrlKey=link.view.search.url).

1. Download [Utah_1889.jpg, an historical image of Utah](images/Utah_1889.jpg) from the GitHub course repo.
2. Georeference Utah_1889. Use the National Geographic World Map as your basemap to georeference this map. This can be added by choosing Add Basemap from the dropdown function of the Add Data button, or the File→Add Data menu. In the Add Basemap dialog box, try the National Geographic World Map.
3. Digitize the permiter of Great Salt Lake.
4. Convert the shapefile to a KML for viewing in Google Earth. Please note that you need to export the map using the symbology that you want Google Earth to display (try hollow, with a 2 pt border). This can be done by using the "Layer to KML" tool in ArcMap. 
5. Once the KML layer is exported, double-click on it to launch Google Earth and view the file. Once you are in Google Earth you have access to imagery since 1970 by clicking on the button that looks like a clock with an arrow pointing counterclockwise.

Write a lab report that includes: 

* **Introduction:** State your research question: How much has the Great Salt Lake in Utah changed since 1889?
* **Data and Methods:** The data used for this assignment is stated in the first part of the assignment. Address your methods for georeferencing and digitizing in your report. Make sure to state and justify why you chose specific mapping options.
* **Results:** Report your RMSE and an evaluation of your success in digitizing a map from 1889
* **Discussion:** Interpret your results by qualitatively evaluating how the Great Salt Lake has changed since 1889.
* **Tables and Figures:** Insert all tables and figures (including maps) at the end of the report, each on a separate page, with a label (e.g. Figure 1). Be sure to cite each table and figure included in the body of the report text.
