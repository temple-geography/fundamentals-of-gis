**Learning Objective**

To introduce raster data, including elevation and land cover data sets.
To introduce raster data manipulation, including mosaic, mask,
projection. To introduce raster analysis, including terrain
visualization and analysis, as well as map algebra. As a demonstration,
you will work with elevation and land cover data for Delaware County,
Pennsylvania.

First, complete the tutorial by following the steps below. Then, using
the skills you’ve learned in the tutorial, complete the assignment given
following the tutorial.

# TUTORIAL

## Acquiring the Data

We will work with elevation and land cover data, as well as municipal
boundary data, for Delaware County, Pennsylvania.

**First, acquire the municipalities data.**

From [PASDA](http://www.pasda.psu.edu/), use the search function to find
and download the following file:

  - Pennsylvania municipality boundaries (2017)

**Next, acquire the elevation data.**

We will download raster elevation data from [The National
Map](https://nationalmap.gov/) (<https://nationalmap.gov/>), the US
federal data geoportal administered by the US Geological Survey (USGS).

1.  > Go to The National Map, click on Find Data + View & Download.
    > Then, under GIS Data click on Download GIS Data to load the map
    > viewer.

2.  > On the left side of the window, under Dataset click on Elevation
    > Products (3DEP) and then check the box for 1 arc-second DEM.
    > Uncheck the other boxes.

![](images/Lab7Fig1.png)

3.  > In the Search location text bar type in Delaware County,
    > Pennsylvania and zoom to Delaware County.

![](images/Lab7Fig2.png)

4.  > In the pop up box for Delaware County click on the Find Products
    > link.

![](images/Lab7Fig3.png)

5.  > On the left side of the window click on Download to download the
    > DEM in a zip format (38.66 MB).

> **Because the DEM data do not adhere to county boundaries, we will
> need to download an additional DEM data set to cover the entirety of
> Delaware County.**

6.  > Repeat the steps above, but search for and download the 1
    > arc-second DEM for Montgomery County, Pennsylvania.

These steps should yield two zip files:

  - USGS\_NED\_1\_n40w076\_ArcGrid.zip

  - USGS\_NED\_1\_n41w076\_ArcGrid.zip

**Next, acquire the land cover data.**

1.  > While still in The National Map, click the Return to Search button
    > at the top of the left side of the window to conduct a new
    > Datasets search.

2.  > Uncheck the Elevation Products (3DEP) box.

3.  > Check the National Land Cover Database (NLCD) box. Then check the
    > National Land Cover Database (NLCD) – 2011 box.

![](images/Lab7Fig4.png)

4.  > In the Search location bar search again for Delaware County,
    > Pennsylvania and click on the Find Products on the Delaware County
    > pop-up box.

5.  > To the right of the NLCD 2001 Land Cover (2011 Edition, amended
    > 2014), 3x3 Degree data set, click on Download to download the zip
    > file:

<!-- end list -->

  - NLCD2011\_LC\_N39W075.zip

Move all files to your workspace folder and unzip each of the 4 data
files (2 elevation grids, land cover, and municipalities).

## Explore the Data 

Open ArcCatalog and explore each of the downloaded data sets. Note that
raster data are symbolized in the catalog tree using a grid symbol.

![](images/Lab7Fig5.png)

View the land cover raster (with tif file extention). Zoom in so you can
see individual grid cells.

![](images/Lab7Fig6.png)

Now view the land cover attribute table. The Value field in the table,
and the different colors in the image, correspond to different land
cover categories, which are described on NLCD web page:
<https://www.mrlc.gov/nlcd11_leg.php>.

View each of the elevation grids. Note that there are no attribute
tables because the grids are a floating point data type (i.e. continuous
data). The value of each grid cell is the altitude in meters above mean
sea level.

## Mosaic the Elevation Data

Open ArcMap and add the two elevation grids. They should appear adjacent
to each other.

![](images/Lab7Fig7.png)

Here we will use the mosaic operation to merge the two grids into a
single grid.

1.  > Open ArcToolbox and go to Data Management Tools→Raster→Raster
    > Dataset→Mosaic to New Raster.

2.  > In the Mosaic to New Raster dialog box, for Input Rasters choose
    > the two elevation grids, for Output Location navigate to your
    > workspace folder. For Raster Dataset Name with Extension enter
    > elev\_mo. For Number of Bands enter 1. Press OK.

![](images/Lab7Fig8.png)

3.  > View the new grid. It should appear as the two original grids
    > merged together.

4.  > Remove the two original elevation grids, leaving the elev\_mo
    > grid.

## Mask the Raster Data to Delaware County

Here, we will extract the elevation data for Delaware County.

1.  > Add the municipalities layer to ArcMap with the elevation mosaic
    > grid.

2.  > Use a Select by Attribute operation to select Delaware County
    > municipalities from the municipalities data. In the attribute
    > table, the County field value=23 is Delaware County.

3.  > Export the Delaware County municipalities to its own shapefile
    > named del\_cnty.

4.  > Remove the original municipalities layer from ArcMap.

![](images/Lab7Fig9.png)

5.  > Turn on the Spatial Analyst raster processing extension by going
    > to the Customize menu and Extensions and checking the box for
    > Spatial Analyst (if it is unchecked).

6.  > Open ArcToolbox and go to Spatial Analyst Tools→Extraction→Extract
    > by Mask.

7.  > In the Extract by Mask tool, for Input raster choose the elevation
    > mosaic grid and for Input raster or feature mask data choose
    > del\_cnty. Name the output raster del\_elev.

8.  > Turn off all the layers except del\_elev and view the new raster.

![](images/Lab7Fig10.png)

**Now, we will mask the land cover data to Delaware County, but we will
need to open a new ArcMap session because the land cover grid is in a
different CRS.**

1.  > In ArcMap go to the File menu and choose New. Click OK. Do not
    > save changes.

2.  > Add the land cover grid with the tif file extension.

3.  > Add the del\_cnty layer.

4.  > Repeat the steps above to mask the land cover data to Delaware
    > County. Call the output raster del\_land.

![](images/Lab7Fig11.png)

5.  > Close ArcMap and do not save.

## Reproject the Data

View the del\_elev and del\_land grids in ArcCatalog. They are clearly
different shapes because they are in different CRSs, as you can confirm
by going to the Layer Properties of each. Here, we will reproject all
three data sets (the del\_elev and del\_land grids, and the del\_cnty
shapefile) into UTM.

1.  > In ArCatalog, reproject the del\_cnty shapefile into
    > NAD\_1983\_UTM\_Zone 18N.

2.  > Reproject for raster data uses a different tool. Go to
    > ArcToolbox→Data Management Tools→Projections and
    > Transformations→Raster→Project Raster.

3.  > The tool dialog box is similar to the vector Project tool. Convert
    > both the del\_elev and the del\_land rasters to
    > NAD\_1983\_UTM\_Zone 18N. Name the new grids del\_elev\_utm and
    > del\_land\_utm, respectively.

## Creating a Hillshade

A hillshade is a grid that encodes a hypothetical reflectance value off
an elevation surface given the position of a virtual light source. It
allows for the visual representation of the terrain as though you were
viewing the surface from an airplane.

1.  > Open ArcMap and add the del\_elev\_utm grid.

2.  > To create a hillshade grid, go to ArcToolbox→Spatial Analyst
    > Tools→Surface→Hillshade. For Input raster choose del\_elev\_utm.
    > Name the output hill\_1. Leave the other defaults and click OK.

3.  > In the resulting hillshade you should be able to see the more
    > rugged terrain in the north and west of the county and the stream
    > valleys which flow south towards the Delaware River.

![](images/Lab7Fig12.png)

 

4.  > You can also exaggerate the vertical height to improve the
    > visualization of the terrain. Perform another hillshade but this
    > time change the Z factor to 2 or 3. Experiment with other
    > parameter changes in the hillshade, including the virtual location
    > of the light source by tweaking Altitude and Azimuth settings.
    > Examine the effects of changing parameters on the resulting
    > hillshade layer.

## Visualizing Terrain

Impressive visualizations of terrain can be easily generated by using
the elevation and hillshade grids together. Turn off everything except
the elevation and your preferred hillshade grids from your experiments
in the last section, and order your layers so that the hillshade is ‘on
top’.

1.  > Open the Layer Properties for the elevation grid and select the
    > Symbology tab. (Notice how it differs slightly from the Symbology
    > tab for vector layers that we have been working with in previous
    > lab exercises.) Choose a multi-color color ramp like the one shown
    > here:

![](images/Lab7Fig13.png) 

2.  > Now open the Layer Properties for the `hillshade` grid and go to
    > the Display tab. Change the transparency to 30%. Close the
    > Properties dialog. Your display should now look something like
    > this:

![](images/Lab7Fig14.png) 

3.  > You can also add land use to further enhance the visualization.
    > Add the del\_land\_utm grid to ArcMap and move it between the
    > hillshade and elevation grids in the TOC. Experiment with
    > different transparency settings for the land use and hillshade
    > grids so that both can be seen simlutaneously. (Start with land
    > use transparency=40% and hillshade transparency=50%).

![](images/Lab7Fig15.png)

4.  > Now try displaying only the developed area in the land use grid to
    > show where the cities are in relation to the elevation
    > information. You can change this setting in the Symbology tab of
    > the land use Layer Properties. Select all the land cover
    > categories except for the Developed, High Intensity land covers
    > and press Remove. You may want to adjust the color setting, or the
    > transparency setting in the Display tab to make it less
    > transparent (say, 20%). Press OK.

![](images/Lab7Fig16.png)

## Calculating Slope

The slope operation allows you to derive slope from elevation. Slope is
calculated as rise over run, or the gradient of change in elevation, and
can be measured in degrees or percent.

1.  > Turn off the layers except for del\_elev\_utm.

2.  > Go to ArcToolbox→Spatial Analyst Tools→Surface→Slope. For Input
    > raster choose del\_elev\_utm. Name you grid slope. Leave the
    > Output measurement as the default (degree). Press OK.

3.  > Examine the resulting slope grid. Note that the steepest slopes
    > occur along the streams.

![](images/Lab7Fig17.png)

## Summarizing Grid Data using a Zonal Function

A zonal function allows you to summarize the values of raster grid cells
in zones specified in a second file. The zone file can be a raster or
vector format. As an example we will summarize the slope by land use
category in Delaware County.

1.  > Turn off all the layers but the slope and land use grids.

2.  > Go to ArcToolbox→Spatial Analyst Tools→Zonal→Zonal Statistics as
    > Table. For Input raster or feature zone data choose
    > del\_land\_utm. The Zone field should default to Value, since this
    > is the only data associated with this raster. For Input value
    > raster choose the slope grid. Name the output lu\_slope. Press OK.

3.  > Open the resulting table. (If you don’t see the new table, click
    > on the List by Source tab in the TOC.) The zonal function
    > generates a table in which each record is a zone (here, land use)
    > and the fields contain information on various aggregate functions
    > such as the minimum, maximum, range, mean, standard deviation, and
    > sum (which is not applicable here) of the values (here, slope) for
    > each zone. For example, the MEAN field reports the mean slope
    > value for each land use category. The mean slope for Developed,
    > High Intensity land (Value=24) is 1.55°, whereas for Deciduous
    > Forest land (Value=41) the mean slope is 6.41°.

![](images/Lab7Fig18.png)

4.  > The COUNT field is the number of grid cells in that zone (here,
    > land use) and the AREA field is the area. For instance, the number
    > of grid cells in the Developed, High Intensity category in
    > Delaware County is 28,923, and the total area of Developed, High
    > Intensity land is 26,030,700 square meters (since the data are in
    > the UTM CRS, which uses meters as units).

It is also possible to use a vector data layer as the zone layer in a
zonal function. Here, we will summarize the slope by municipality in
Delaware County.

1.  > Add the del\_cnty\_utm layer to ArcMap. Repeat the Zonal
    > Statistics as Table function. For Input raster or feature zone
    > data choose del\_cnty\_utm. For Zone field, choose MUNICIPAL1, as
    > this is the name of each municipality (we want an output table
    > where each record represents a unique municipality). For Input
    > value raster choose the slope grid. Name the output mun\_slope.
    > Press OK.

2.  > View the output table. It should have 47 records – one for each
    > municipality.

Let’s visualize the results by creating a choropleth map of the mean
slope by municipality.

1.  > Join the mun\_slope table to the del\_cnty\_utm attribute table
    > using the MUNICIPAL1 field to facilitate the join.

2.  > Create a choropleth map of the MEAN field (i.e. the mean slope of
    > each municipality). Use a quantile classification and choose a
    > sequential color scheme. You can see that the municipalities with
    > the average steepest slopes occur in the central and western part
    > of the county.

![](images/Lab7Fig19.png)

# ASSIGNMENT

## Objective

Flooding in Delaware County is a problem, particularly in the
southeastern portion where several streams flow into the Delaware River.
Flooding is made worse by steep slopes and the presence of impervious
surface cover (i.e. developed land cover surfaces which do not allow
water to infiltrate, such as pavement and rooftops), both of which
contribute to rapid, high volumes of runoff during storm events.

The objective is to identify municipalities that might be key
contributors to flooding in Delaware County. Specifically, answer the
following questions:

Which municipalities in Delaware County, Pennsylvania have a *median*
percent impervious surface area of at least 50%?

Which municipalities in Delaware County, Pennsylvania have a *mean*
percent slope of at least 5 degrees?

## Deliverables

**Turn in a report in the format described in the syllabus.**

Be sure to include the following information:

1.  A list of the municipalities which have a median percent impervious
    surface area of at least 50%.

2.  A list of the municipalities which have a mean percent slope of at
    least 5 degrees.

3.  A map of the terrain that simultaneously shows the hillshade and
    elevation with the grid cells with a percent impervious surface
    <span class="underline">\></span> 50% overlain.

4.  A choropleth map of the median percent impervious surface by
    municipality.

The **Introduction** section should state the research objective and the
relevant criteria for identifying the municipalities.

The **Data and Methods** section should state the data sets used in the
analysis, from where those data were acquired, and the GIS operations
employed.

The **Results** section should state the results (i.e. the lists of the
municipalities that meet each of the criteria). The maps should be cited
in the text here.

The **Discussion** section should state an interpretation of the results
(i.e. what parts of the county have steeps slopes or high impervious
surface percentages), limitations of the analysis, and how the analysis
could be improved or expanded.

The **Tables** **and Figures** section should contain the maps, each on
a separate page with a caption. The maps should be cited in the text.

## Getting Started

You can use some of the products you have created in the tutorial for
the assignment. You will also need to download a new raster data set on
impervious surface.

1.  Download the 2011 NLCD Impervious Surface data from The National Map
    (you can follow the same directions you used to download the land
    cover data, above, but instead of the land cover data download the
    Percent Developed Imperviousness data from the available products).
    The value of each grid cell in the impervious surface grid is the
    percentage of the grid cell covered by impervious surface. Mask the
    impervious surface data to Delaware County, and convert it to the
    UTM CRS.

2.  Use the elevation grid and the hillshade grid you created, in
    combination with the impervious surface grid, to generate a map of
    the terrain that simultaneously shows the hillshade and elevation
    with the percent impervious surface layer overlain.

3.  Use the output table from the Zonal Statistics as Table function you
    completed in the tutorial using the municipalities (zone layer) and
    the slope grid (value grid) to identify the municipalities which
    have a mean percent slope of at least 5 degrees.

4.  Use a Zonal Statistics as Table function using the municipalities
    layer (as the zone layer) and the impervious surface grid (as the
    value grid) to generate a table from which you can identify the
    municipalities which have a median percent impervious surface area
    of at least 50%.

5.  Join the output table from the previous Zonal Statistics as Table
    function to the municipalities attribute table to generate a
    choropleth map of the median percent impervious surface by
    municipality.
