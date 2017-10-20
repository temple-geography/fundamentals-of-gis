**Purpose:** The purpose of this exercise is to introduce the basics of georeferencing and digitizing in ArcGIS.

Data
====

The data for this tutorial includes an historical map image hosted on the course GitHub repo, and two modern GIS layers which you will download from [MassGIS](https://www.mass.gov/service-details/massgis-data-layers), Massachusetts' spatial data repository. The towns and ponds layers we are interested in are in ZIP archives (linked below) which contain other layers. Download and unzip the archives, but keep in mind that we won't be using all of the data downloaded, and make sure to add the correct layer when directed to.

-   [Worcester\_towns.jpg](https://github.com/temple-geography/fundamentals-of-gis/raw/master/images/Worcester_towns.jpg): Follow the link save it to your workspace. This is a JPEG image of an 1871 map of towns in Worcester County, Massachusetts, downloaded from DavidRumsey.com. [Click for more information about this image.](https://www.davidrumsey.com/luna/servlet/detail/RUMSEY~8~1~26368~1100042:Worcester-County-?sort=Pub_List_No_InitialSort%2CPub_Date%2CPub_List_No%2CSeries_No&qvq=q:towns%2Bin%2Bmassachusetts;sort:Pub_List_No_InitialSort%2CPub_Date%2CPub_List_No%2CSeries_No;lc:RUMSEY~8~1&mi=29&trs=294)
-   `TOWNS_POLY.shp`: A shapefile of the town boundaries in Massachusetts. It is included in the [Community Boundaries (Towns)](http://www.mass.gov/anf/research-and-tech/it-serv-and-support/application-serv/office-of-geographic-information-massgis/datalayers/towns.html) bundle.
-   `MAJPONDS_POLY.shp`: A shapefile of major ponds in Massachusetts. It is included in the [Major Ponds and Major Streams](http://www.mass.gov/anf/research-and-tech/it-serv-and-support/application-serv/office-of-geographic-information-massgis/datalayers/majhd.html) bundle.

Georeferencing
==============

Georeferencing is the process of aligning data with real-world coordinates. In this tutorial we will geoference a historic map of towns in Massachusetts to a modern GIS layer of town boundaries, and we will digitize the lakes in the Town of Auburn and compare them to a current shapefile of lakes. This process will take a few steps.

A video of this georeferencing tutorial is available at <https://drive.google.com/file/d/0B_rk0_Y4N6QzdEg3RmFSY2tjUUU/view> and a brief description of the steps follow. You might find it useful to review the steps below and watch the video before beginning the tutorial.

1.  Open (using the system image viewer, not ArcMap) and review the JPEG image of towns in Worcester, Massachusetts, 1871. Close the image.
2.  Open ArcMap.
3.  Add `TOWNS_POLY.shp` to the map canvas.
4.  Set the polygon fill to hollow, so that you can see the town borders, but also see the historical map (which we are about to add).
5.  Add the 1871 towns map to ArcMap.
    1.  Use Add Data as if this were a shapefile or other GIS layer. Even though this is just an image file, not a GIS data layer, the file will show up in the Add Data dialog box as a suitable data source.
    2.  You will get the by now familiar Unknown Spatial Reference warning. This is OK! This is the problem that you are about to fix! Hit OK to dismiss the dialog.
    3.  ***The image will not appear in the map initially.*** Since it is not a spatial layer, ArcGIS does not know how to display it. Nonetheless, `Worcester_towns.jpg` will be listed in the TOC, so you will know that it is there, and be able to change Layer properties in the next step. Even though you can't see an image, keep following the instructions. You won't be able to see the image until you turn on the Georeferencing toolbar and begin that process.
6.  Open the Layer Properties for the Worcester towns layer and select the Display tab.
7.  Change the Resample during display setting to Cubic Convolution. This will improve the quality of the display.

The data are now available in your map document and you are ready to begin Georeferencing.

1.  Go to the Customize→Toolbars menu. A long list of toolbars will appear.[1] Turn on (select in the menu) the Georeferencing toolbar. The Georeferencing toolbar will appear floating over your map canvas. You can drag this to a position that you like, including docking it to the top (among the other toolbars), bottom, or sides of the ArcMap window.
2.  As we have only one image, Worcester\_towns.jpg, in this map document, it should already appear in the dropdown of the layer to be georeferenced. If for some reason it is not, select the layer name in the dropdown.
3.  From the Georeferencing menu that appears in the toolbar, select Fit to Display. The JPEG image will now appear in the visible part of the map canvas, along with the rest of the layers you added.

You are now ready to begin placing ground control points (GCPs). The GCPs link coordinates from the map (already referenced) with the image of the historical map.

1.  In the Georeferencing toolbar, click the Add Control Points button ![Add Control Points](images/ArcMapAddControlPointsButton.png).
2.  Place the first GCP:
    1.  Look at the modern towns layer and the map to identify (by shape, or possibly by name) a town that is the same in both data sources.
    2.  Click on a corner of the town in the JPEG image. ***NOTE:** You **must** click on the image **first***.
    3.  Click on the matching corner of the town in the `TOWNS_POLY` layer. The image will immediately reposition itself so that that point in the two data sources are aligned. The scale (size of the towns) will (probably) still be off.
    4.  Repeat the process for another (perhaps the opposite) corner of the same town. The image will now be rescaled to approximately the same size as the modern towns layer. It won't be perfect, but even with only two GCPs, it should now be visibly obvious that the two data sources are slightly different versions of the same real-world entities.
3.  Place additional GCPs. As you place additional points, the image will align more and more closely to the modern data.
4.  In addition to visually assessing the fit, you should also evaluate the fit using the residuals and **root mean square error (RMSE)**. Click the View Link Table button ![View Link Table](images/ArcMapViewLinkTableButton.png) on the Georeferencing toolbar. The residual tells you how much discrepancy lies between the reference data (modern towns layer) and the JPEG image. Values close to 0 indicate that you are doing a good job. If the RMSE reaches 0, you have perfect fit. You can delete control points that have a very large residual, and you may find that the fit will be improved by deleting these points.
5.  Continue placing additional GCPs. Ideally, your control points will be well-distributed across the map. We can't know the number of required GCPs in advance. The process is iterative. As you place additional points, you will evaluate the fit, erase the ones with a poor fit, add new points to improve the fit, etc. **NOTE:** The next section of the lab will focus on digitizing features in the town of Auburn, so please prioritize the fit in Auburn.
6.  When you are satisfied with your GCPs, you will want to save the JPEG as a new image.
    1.  Select Rectify from the Georeferencing menu.
    2.  Change the resample type to Bilinear Interpolation.
    3.  Select the folder (Output Location) where you would like to save the image. **NOTE:** The Select Workspace dialog is somewhat confusing. Do *not* try to enter a filename. Just select the folder and hit Add.
    4.  Change the image Name (if you are not happy with the default name).
    5.  The Format should be TIFF. (If it is some other type, change it to TIFF.)
    6.  TIFF images can be very large. Change the Compression Type to JPEG. Compression Quality can stay at the default value (which should be 75).
    7.  Hit Save.

Digitizing
==========

For historical research, the researcher/analyst will often need to **digitize** features by referring to an historical map. Now that you have finished geoferencing the towns map, you can begin digitizing features drawn on the map. We are going to digitize the lakes in the town of Auburn and compare the historical locations of these lakes to the current water bodies taken from a modern GIS layer.

A video of this digitizing tutorial is available at <https://drive.google.com/file/d/0B_rk0_Y4N6QzWFFDclRUcnE3TkU/view> and a brief description of the steps follow. You might find it useful to review the steps below and watch the video before beginning the tutorial.

1.  Add the georeferenced TIFF (the result of the previous section) to the map document.
2.  Open the ArcCatalog pane in ArcGIS and navigate the folder tree to get to your working folder.
3.  Right-click in your folder and select New→Shapefile.
4.  Create the settings for your shapefile as follows:
    1.  Name: `Lakes_auburn`
    2.  Feature type: polygon
    3.  Spatial reference: Select the Edit button, then Import, and in the following window, select the `TOWNS_POLY` shapefile. The new (empty) shapefil) will now have the same spatial reference information as the `TOWNS_POLY` shapefile. Any features that you create will use coordinates from that coordinate reference system.
5.  Click OK and the new shapefile will be created in the selected folder and added to the TOC. (If it does not get added to the map automatically, you can use the Add Data button as usual.)
6.  Display the Editor toolbar.
7.  In the Editor toolbar, select Editor→Start Editing.
8.  Select your newly created 'Lakes\_auburn' shapefile to begin editing.
    1.  You will see a new pane appear on the right. Your new shapefile should appear.
9.  Select the shapefile, and look at the bottom of the pane. A number of Construction tools will appear. Select the Polygon tool.
10. Begin tracing the outline of one of the lakes in the town of Auburn. Each click will add a **vertex** to the polygon. Draw the polygon with a series of closely spaced clicks. Where the boundary is curved, you will have to add more vertices to follow the curve. Where the boundary is straight, the vertices can be more spread out.
11. When you have finished one polygon, double-click to complete the sketch. The polygon will be outlined in teal, indicating that the shape is complete.
12. Return to the Editor toolbar and select Editor→Save Edits so that you do not lose any work.
13. Digitize all of the lakes in the town of Auburn. When you are finished, save your edits once again and chose Editor→Stop Editing.
14. Add the `MAJPONDS_POLY.shp` layer to compare the locations of lakes in 2017 and 1871.

Assignment
==========

In 2016 there were a number of articles about the shrinking Great Salt Lake <https://earthobservatory.nasa.gov/IOTD/view.php?id=88929> in Utah.

How much has the Great Salt Lake in Utah changed since 1889? You will start with an historical image of Utah. You will georeference the image, digitize the perimeter of the the Great Salt Lake, and compare the digitized lake to a modern data source. As with the Worcester towns historical map, [this data is downloaded from DavidRumsey.com](https://www.davidrumsey.com/luna/servlet/detail/RUMSEY~8~1~37442~1210316:Utah-?showTipAdvancedSearch=false&showShareIIIFLink=true&showTip=false&helpUrl=https%3A%2F%2Fdoc.lunaimaging.com%2Fdisplay%2FV73D%2FLUNA%2BViewer%23LUNAViewer-LUNAViewer&title=Search+Results%3A+List_No+equal+to+%272094.058%27&fullTextSearchChecked=&dateRangeSearchChecked=&advancedSearchUrl=https%3A%2F%2Fdoc.lunaimaging.com%2Fdisplay%2FV73D%2FSearching%23Searching-Searching&thumbnailViewUrlKey=link.view.search.url).

1.  Download [Utah\_1889.jpg](https://github.com/temple-geography/fundamentals-of-gis/raw/master/images/Utah_1889.jpg), an historical image of Utah from the GitHub course repo.
2.  Georeference Utah\_1889. Use the National Geographic World Map as your basemap to georeference this map. This can be added by choosing Add Basemap from the dropdown function of the Add Data button, or the File→Add Data menu. In the Add Basemap dialog box, try the National Geographic World Map.
3.  Digitize the perimeter of Great Salt Lake.
4.  Save the shapefile as a Layer file and then convert to KML file for viewing in Google Earth. This will allow you to visually compare the historic extent of the Great Salt Lake to its current extent. A layer in ArcGIS references a spatial data set and includes a set of rules for how it should be displayed. (Think how a layer in ArcGIS is both a pointer to a data source and rules about how to display it.) To create the KML file:
    1.  Change the symbology of the shapefile to a hollow fill with a 2 point border.
    2.  Right-click the layer in the TOC and select Save as Layer File. Save it to your working folder.
    3.  Use the Layer to KML tool to create a new KMZ file (a zipped .KML file). This tool is found in ArcToolbox under Conversion Tools→To KML→Layer to KML. The input file will be the Layer File that you just saved.
5.  Once the KML layer is exported, double-click it in Windows File Explorer. This will launch Google Earth and view the file. Once you are in Google Earth you can access imagery since 1970 by clicking on the button that looks like a clock with an arrow pointing counterclockwise.

Write a lab report that includes:

-   **Introduction:** State your research question: How much has the Great Salt Lake in Utah changed since 1889?
-   **Data and Methods:** The data used for this assignment is stated in the first part of the assignment. Address your methods for georeferencing and digitizing in your report. Make sure to state and justify why you chose specific mapping options.
-   **Results:** Report your RMSE and an evaluation of your success in digitizing a map from 1889
-   **Discussion:** Interpret your results by qualitatively evaluating how the Great Salt Lake has changed since 1889.
-   **Tables and Figures:** Insert all tables and figures (including maps) at the end of the report, each on a separate page, with a label (e.g. Figure 1). Be sure to cite each table and figure included in the body of the report text.

[1] You can also show the list of toolbars to display or hide by right-clicking anywhere in the toolbar area, i.e. on an visible toolbar or in the gray space next to a visible toolbar.
