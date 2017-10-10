**Purpose:** The purpose of this exercise is to introduce the basics of georeferencing and digitizing in ArcGIS.

Georeferencing
==============

Georeferencing is the process of aligning data with real-world coordinates. In the tutorial we will geoference a historic map ofcounties in Massachusetts to the current county map, and we will lakes in the Town of Auburn and compare them to a current shapefile of lakes. This process will take a few steps.

A video of this georeferencing tutorial is available at <https://drive.google.com/file/d/0B_rk0_Y4N6QzdEg3RmFSY2tjUUU/view> and a brief description of the steps follow. You might find it useful toreview the steps below and watch the video before beginning the tutorial.

Copy over your data to your workspace.

This folder contains the following data:

1.  Worcester\_towns: An image (.jpg) of a 1871 map of towns in Worcester County, Massachusetts downloaded from DavidRumsey.com. See<https://www.davidrumsey.com/luna/servlet/detail/RUMSEY~8~1~26368~1100042:Worcester-County-?sort=Pub_List_No_InitialSort%2CPub_Date%2CPub_List_No%2CSeries_No&qvq=q:towns%2Bin%2Bmassachusetts;sort:Pub_List_No_InitialSort%2CPub_Date%2CPub_List_No%2CSeries_No;lc:RUMSEY~8~1&mi=29&trs=294> for more information about this image.

2.  Towns\_polyA shapefile of the current town boundaries in Worcester County downloaded from MassGIS, the state’s data spatial data repository. See<http://www.mass.gov/anf/research-and-tech/it-serv-and-support/application-serv/office-of-geographic-information-massgis/datalayers/towns.html> for more information about this shapefile.

3.  Majponds\_poly: A shapefile of major ponds in Massachusetts is also downloaded from MassGIS

4.  Open the 1871 jpeg of counties in Massachusetts and review the image.
5.  Double-click on the file titled MA.mxd and you will see the 2017 town boundaries in Massachusetts.
6.  Click the “add data” button and add the 1871 map to ArcMap.
    1.  You may also receive a message indicated that the layer does not have a spatial reference. Click ok. This is the problem that you are about to fix!
    2.  The .jpg will not appear in the map, but you will be able to see the name listed in the Table of Contents, so you will know that it is there.
7.  Right click on the .jpg in the Table of Contents.
8.  Select Properties.
9.  Select the Display tab.
10. Change the Resample during display setting to Cubic Convolution. Also change the Transparency to 50%.
    1.  This setting will improve the quality of the display. Click Ok.

Now you are ready to begin Georeferencing.

1.  In the grayspace next to the tool bar, right click and a long list of tools will appear (listed in alphabetical order). Scroll down until you see 'Georeferencing.' Select Georeferencing and the tool bar will appear.
2.  Change the layer to be georeferenced to your Worcester\_towns.jpg.
3.  Select the Georeferencing drop down and select 'Fit to Display.'
    1.  This will bring your .jpg into the same space as the rest of the georeferenced layers, making it easier to georeference .jpg.

You are now ready to begin placing ground control points (GCPs). The GCPs link coordinates from the map (already referenced) with the .jpg.

1.  In the 'Georeferencing' toolbar select 'Add Control Points'
2.  Place the first GCP by **First** clicking on the .jpg and then clicking on the correspoing shapefile.
    1.  Ideally your control points will be well distributed across the map, however, you will note that this process is iterative and there isn’t a preset series of control points. Rather, you will need to place a seriesof points, evaluate the fit, erase the ones with a poor fit, add new points to improve the fit, etc.
    2.  For this map, you might find it easiest to place the first two points on the corners of one town that you can easily match to help resize and bring the maps within a similar location.
    3.  The maps will auto-adjust each time you place a point, so you will be able to visually assess the fit. You should also evaluate the fit using the residuals and RMSE by opening the Link Table by selecting the button next to the 'Control Point' button on the Georeferencing ToolBar.
    4.  The residual tells you how much discrepancy lies between the reference map and the .jpg. Values close to 0 indicate that you are doing a good job. If the RMSE reaches 0, you have perfect fit. You can delete control points that have a very large residual, and you may find that the fit will be improved by deleting these points.
    5.  The next section of the lab will focus on digitizing features in the town of Auburn, so please prioritize the fit in Auburn.
3.  Once you are done, select Rectify from the Georeferencing Tool Bar.
    1.  The following window will allow you to save your georeferenced .jpg as a new file.
4.  Change the resample type to bilinear Interpolation and specify the folder to which you would like the file to be saved.
    1.  You do not need to double-click inside the folder, but click once to highlight the folder.

Digitizing
==========

After you have finished geoferencing, you can begin digitizing the features drawn on the maps. We are going to digitize the lakes in the town of Auburn and compare the locations of these lakes to the current water bodies in the town.

A video of this digitizing tutorial is available at <https://drive.google.com/file/d/0B_rk0_Y4N6QzdEg3RmFSY2tjUUU/view> and a brief description of the steps follow. You might find it useful to review the steps below and watch the video before beginning the tutorial.

1.  Add your georeferenced .tiff to the map document.
2.  Open the ArcCatalog pane in ArcGIS. Right click in your folder and select 'New→' 'Shapefile'.
3.  Create the settings for your shapefile as follows:
    1.  Name: Lakes\_auburn
    2.  Feature type: polygon
    3.  Spatial reference: Select the Edit button, then Import and in the following window, select the 'TOWNS\_POLY' shapefile. This will import the same spatial reference as the 'TOWNS\_POLY' shapefile.
4.  Click ok and your new shapefile will be added to your workspace. If it does not add automatically, you can add the new shapefile using the ‘add data’ button.
5.  Right click in the grey space near the file menu and select the 'Editor' tool bar.
6.  Select Editor and then 'Start Editing'
7.  Select your newly created 'Lakes\_auburn' shapefile to begin editing.
    1.  You will see a new pane appear on the right. Your new shapefile should appear.
8.  Select the shapefile, and look at the bottom of the pane. You should see a number of 'Construction' tools appear. 'Select Polygon'.
9.  Begin tracing the outline of one of the lakes in the town of Auburn with a series of clicks that are not too far apart:
10. When you have finished one polygon, double-click to complete the sketch.
    1.  The polygon will out outlined in teal, indicating that the shape is complete.
    2.  Each time you complete a feature (e.g. polygon) return to the Editor Tool Bar, and select Save Edits from the drop down so that you do not lose any work.
11. Digitize all of the lakes in the town of Auburn and when you are finished, save your edits once again and chose ‘stop editing’ from the Editor dropdown.
12. Add the “majpond\_poly.shp to compare the locations of lakes in 2017 and 1871.

Assignment
==========

In 2016 there were a number of articles about the shrinking Great Salt Lake <https://earthobservatory.nasa.gov/IOTD/view.php?id=88929> in Utah.

How much has the Great Salt Lake in Utah changed since 1889?

1.  Georeference Utah\_1889, downloaded from <https://www.davidrumsey.com/luna/servlet/detail/RUMSEY~8~1~37442~1210316:Utah-?showTipAdvancedSearch=false&showShareIIIFLink=true&showTip=false&helpUrl=https%3A%2F%2Fdoc.lunaimaging.com%2Fdisplay%2FV73D%2FLUNA%2BViewer%23LUNAViewer-LUNAViewer&title=Search+Results%3A+List_No+equal+to+%272094.058%27&fullTextSearchChecked=&dateRangeSearchChecked=&advancedSearchUrl=https%3A%2F%2Fdoc.lunaimaging.com%2Fdisplay%2FV73D%2FSearching%23Searching-Searching&thumbnailViewUrlKey=link.view.search.url>, and digitize the perimeter of the lake. Use the National Geographic World Map as your basemap to georeference this map. This can be added by choosing the dropdown function of the ‘add data’ button in ArcGIS (choose Add Basemap – try the National Geographic World Map).

2.  Convert the shapefile to a KML and view it in Google Earth. Please note that you need to export the map in the symbology that you wish to view it in Google Earth (try hollow, with a 2 pt border). This can be done by using the “Layer to KML” tool in ArcMap. Once the layer is exported, double-click on it to launch Google Earth and view the file. Once you are in Google Earth you have access to imagery since 1970 by clicking on the button that looks like a clock with an arrow pointing counterclockwise.

Write a lab report that includes:

-   **Introduction:** State your research question: How much has the Great Salt Lake in Utah changed since 1889?
-   **Data and Methods:** The data used for this assignment is stated in the first part of the assignment. Address your methods for georeferencing and digitizing in your report. Make sure to state and justify why you chose specific mapping options.
-   **Results:** Report your RMSE and an evaluation of your success in digitizing a map from 1889
-   **Discussion:** Interpret your results by qualitatively evaluating how the Great Salt Lake has changed since 1889.
-   **Tables and Figures:** Insert all tables and figures (including maps) at the end of the report, each on a separate page, with a label (e.g. Figure 1). Be sure to cite each table and figure included in the body of the report text.

