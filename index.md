
## Data management in Google Earth Engine

In this second chapter we will learn how to import, operate and display
images and features on Google Earth Engine (GEE). This is the basic
building block for any ecological spatial analysis and with time, this
set of operations and functions will become a simple habit.

In order to learn the basic procedures and functions to work in GEE, we
will develop a simple example. The idea is that after completing this
chapter, you will have a basic understanding on how to change the area
of interest, load and work with different data sets (both, images and
features) and perform some of the most common functions and operations.

This video will guide you through the code of this chapter.

**Data Management in GEE**:
{{< youtube 5alN74QJbqI >}}

### Getting started

The first step in any project that involves coding is to start and save
a script where you can safely keep your work progress. First, using the
forward slash to comment out code, write a header to your script,
providing general information such as, your name and a title. Something
like this:

    /////////////////////////////////
    // My name
    // Getting started with GEE
    /////////////////////////////////

You can also comment chunks of code using a slightly different code:
`/* text */` This can be more handy for when you need to activate and
deactivate several lines of code at the same time.

    /*
    My name
    Getting started with GEE
    */

You can choose the method that is easier for you.

After you completed the header, click on the **New button** on the
Scripts tab of the left panel. From the dropping menu, chose **File**.
This will open a window where you can provide a name and chose the
repository where to save your script. Give scripts proper names. You can
also provide a brief description of your project if desired. Once you
have saved the script for the first time, you just need to click
**Save** (Above the code editor panel) every time you want to save
updates to the script (Fig. 1).

<center>

<figure>
<img src="FigCh2/Figure1.JPG" style="width:60.0%"
alt="Figure 1. Saving a new script." />
<figcaption aria-hidden="true">Figure 1. Saving a new
script.</figcaption>
</figure>

</center>

As you may recall from the introductory chapter, you can also create new
repositories and folders to keep your work organized and share them with
collaborators.

### Defining your study area

The study area for our example is in south-east Asia, encompassing the
south of Myanmar and Thailand. We will use this area to learn the basics
of data manipulation in GEE, but you can pick any place on the globe.

To position the map display on the area of interest, we will use the
function `Map.setCenter()`, for which we need to provide coordinates and
a zoom level (higher numbers indicate larger scale or more zoomed in).
You may recall this from the previous chapter.

    Map.setCenter({lon:99.2, lat:12.6, zoom:6});

The next step is to define a geometry, in this case a rectangle, that
will represent the study area. Click on the rectangle icon from the
geometry tools and draw a rectangle that covers the area of interest
(see Fig. 2). We need to provide a name to the new geometry. Name it
StudyArea.

> Hint: You cannot use spaces for names, so we use capital letters or
> underscores to differenciate words. At the end, it does not matter
> what name you use, but it is good to use names for variables that you
> can recognize.

After you finish, the geometry will appear in your imports (top of the
script editor) as a polygon with four vertices. We will discuss more
about geometries later in this chapter.

<center>

<figure>
<img src="FigCh2/Figure2.JPG" style="width:90.0%"
alt="Figure 2. Rectangle geometry demarcating the study area" />
<figcaption aria-hidden="true">Figure 2. Rectangle geometry demarcating
the study area</figcaption>
</figure>

</center>

You can rename the geometry and save it as a new object using code:

    var Bounds = StudyArea;

You have now defined the study area. Later, we will use this polygon to
clip images and restrict the extent of our analysis.

### Working with images

In GEE, raster data are represented as **Image objects** and are the
main type of data to work with. All images are composed on one or more
bands, where each band has a name, data type, scale, mask and
projection, as well as metadata stored as a set of properties. Let’s
start working with some simple images.

#### Loading images

The first step for our workflow is to import an image. Let’s start
working with a simple image with one band which contains information
about elevation, also known as Digital Elevation Models.

Remember from previous chapter that you can search for places, images,
image collections, and feature collections on the GEE Data Catalog.

To find the elevation data, on the search bar type: ‘SRTM Digital
Elevation Data 30m.’ Now, click on it to display the data description
where you can find information about the temporal availability, data
provider and collection ID (Fig. 3). This data set corresponds to a
digital elevation data from the Shuttle Radar Topography Mission (Farr
et al. 2007) and contains information of elevation at 30m spatial
resolution. You can click **import** to add it to your imports, or you
can use code. We encourage you to use code, so you get more familiarized
with it and also helps you to keep the work organized.

<center>

<figure>
<img src="FigCh2/Figure3.JPG" style="width:90.0%"
alt="Figure 3. SRTM Digital Elevation Data description." />
<figcaption aria-hidden="true">Figure 3. SRTM Digital Elevation Data
description.</figcaption>
</figure>

</center>

The function `ee.Image()` allows you to import image catalogs. To import
the elevation data, we need to provide the directory to the data within
the parenthesis of the function. Remember to assign the image to a new
object using **var** as follows:

    var Elev = ee.Image("USGS/SRTMGL1_003");

You have just imported the elevation data for the entire world.

When working in spatial ecology, frequently, slope, aspect and hillshade
are variables of interest as many species respond positively or
negatively to changes in the terrain surface. Thus, these data sets can
be good predictor variables in several models. We only have information
about elevation, but you can calculate slope, aspect and hillshade and
save them as new objects using other GEE built-in functions.

To calculate all these terrain variables, we will use the built-in
function `ee.Algorithms.Terrains()`.

Let’s apply the function to the elevation image and save it as a new
object.

    var Terrain = ee.Algorithms.Terrain(Elev);

At this point, you should be familiarized with the code syntax to create
objects and use built-in functions in GEE.

Print both elevation images to display the information on the console.

    print(Elev);
    print(Terrain);

You will see that differently from the Elev image that contains one
band, the new image created (Terrain) contains four bands, one for each
variable: elevation, slope, aspect and hillshade. All of these three new
bands where calculated from the elevation data. If you want to know more
about the algorithms used to obtain these results, remember that for all
the Engine built-in functions you can use the API reference on the Docs
tab to see a description and all the parameters used by the function.
The `ee.Algorithms` category contains a list of currently supported
algorithms for specialized or domain specific processing.

Look for the `ee.Algorithms.Terrains()` function in the Docs tab (Fig.
4).

<center>

<figure>
<img src="FigCh2/Figure4.JPG" style="width:70.0%"
alt="Figure 4. View of the ee.Algorithms.Terrains description in the Docs tab." />
<figcaption aria-hidden="true">Figure 4. View of the
ee.Algorithms.Terrains description in the Docs tab.</figcaption>
</figure>

</center>

This exercise shows you the versatility and power of conducting spatial
analysis on GEE. You can in matters of seconds, download elevation data
and calculate slope, aspect and hillshade for the entire world!

In many cases, we are only interested in one specific area and not the
entire planet. We can use the GEE built-in function `clip()` to crop the
image to our area of interest, the polygon we crated earlier.

Using the dot notation, we add the function `clip` to the image object
you want to crop, and specifying in between parenthesis the object
representing the area of interest we previously created.

    var Terrain = Terrain.clip(Bounds);

You have now an image object cropped to the area of interest.

### Displaying data on the map

Often, you will be interested in visualizing the images. To display the
elevation data on the map, you can use the function `Map.addLayer()`. To
the previous code, now add the function to display the data as follows:

    Map.addLayer(Terrain);

Parameters for functions may have default values, and then, you only
need to specify the parameter if you want to change its default value.
For instance, the previous function displays a gray surface. You need to
change the default parameters for the appearance of the image to be able
to visualize elevation change.

In the case of the `Map.addLayers()` function, it contains five
arguments (Fig. 5). The first argument is the ee.Object to display, in
our case the **Elev object**. The second argument are the visualization
parameters, which you can pass to the function as a dictionary. Those
include the bands to display (when you have more than one band in the
image, such as the Terrain image created that has 4 bands), the minimum
and maximum values to display, and colors (Remember that you can also
change those parameters from the Layers button in the display). The
third argument is the name of the layer (a string). The fourth argument
is a Boolean number (i.e., 1 for TRUE or 0 for FALSE) and indicates
whether the layer should be displayed by default or be activated by the
user manually by clicking on the layers tab. The last argument in the
opacity, which goes from 0 (transparent) to 1.

<center>

<figure>
<img src="FigCh2/Figure5.JPG" style="width:70.0%"
alt="Figure 5. Description of the Map.addLayers function." />
<figcaption aria-hidden="true">Figure 5. Description of the
Map.addLayers function.</figcaption>
</figure>

</center>

    Map.addLayer(Elev, {min: 0, max: 1000}, 'DEM', 1);
    Map.addLayer(Terrain, {bands:['elevation'] ,min: 0, max: 1000}, 'Elevation', 1); //Same as before but from the terrain object
    Map.addLayer(Terrain, {bands:['slope'] ,min: 0, max: 40, palette:'white,red'}, 'Slope', 1); //Display slope and add a color palette

Note the difference between an image that was cropped and the one that
was not cropped. Play with the image visualization using the Layers
button in the display.

#### Image collections

We have so far worked with a single image. Now, we are going to access
an entire image collection. Image collections can contain hundreds of
images across long periods of time, such as images from satellites.
Similarly to the elevation image, you can search for image collections.
Let’s access the image collection of Landsat 8. Landsat images come with
different levels of pre-processing. Here we will work with T1, that is
the highest level of pre-processing.

Use the search bar to find the name of the Landsat 8 T1 image collection
and import it using the function `ee.ImageCollection()`.

    var landsat8 = ee.ImageCollection("LANDSAT/LC08/C02/T1")

You just imported the entire image collection of Landsat 8 for the
entire Earth land surface. You could try to print the image collection
to see all the images contained in the list. However, because there are
too many images, printing it will either be very slow, time out, or
return an error.

#### Filtering and sorting

What we need to do to work with image collections is to filter them and
reduce the number of images in the list to the ones that we are
interested in. To do this, we will use filters.

Filters are a set of functions that allows you to select a set of images
based on certain parameters such as position, time windows, numbers,
properties, depending on the type of object you are trying to filter,
such as image collections, feature collections, lists or geometries.
Lets see how this works for image collections.

Let’s start by filtering our image collection by space and by time,
specifying the area and the time frame in which we are interested in
(Remember different satellite products contain images for different time
periods and that information is provided in the data description of each
product). We need to use `filterBounds()` to select images from our area
of interest (the object Bounds) and `filterDate()` to keep only images
from 2019, thus specifying a time window between 2019-01-01 and
2019-12-31. Note how we can add functions to the same line of code using
the dot notation.

    var landsat8 = ee.ImageCollection("LANDSAT/LC08/C02/T1")
        .filterBounds(Bounds)
        .filterDate('2019-01-01', '2019-12-31')

The new object contains only images for which the path overlaps our area
of interest and that were obtained during 2019.

In addition, we are interested in finding images that are cloud-free.
Here we will use another function to solve this problem. All Landsat
images contain information about cloud cover in the metadata. We can use
this information to sort the image collection from lowest cloud cover to
highest cloud cover using the function `sort()`, which operates on the
image metadata. You could apply a new function to the object created
with the previous line of code. You can also simplify things by
concatenating functions in the same line of code, again using the dot
notation.

    var landsat8 = ee.ImageCollection("LANDSAT/LC08/C02/T1")
        .filterBounds(Bounds)
        .filterDate('2019-01-01', '2019-12-31')
        .sort('CLOUD_COVER', true);
        
    print(landsat8) //The image collection contains 240 images

The new image collection is now sorted by the cloud cover percentage,
from lowest to highest. We also see that the collection contains now 240
images.

You can save the first image (lowest cloud cover) as a new object and
display it. This time, you are displaying an image composed by a series
of bands and the final color display depends on the combination of bands
assigned to the tree main colors, red, green and blue. If you assign the
bands corresponding to the three colors, then you obtain a true color
display. You can also try a false color composition or displaying just
the thermal band.

    var first_image = landsat8.first();

    // True color
    Map.addLayer(first_image, {bands: ['B4', 'B3', 'B2'], min: 7000, max: 10000}, 'Landsat8-RGB', 1);

    // False color
    Map.addLayer(first_image, {bands: ['B5', 'B4', 'B3'], min: 7000, max: 18000}, 'Landsat8-FalseColor', 1);

    // Thermal
    Map.addLayer(first_image, {bands: ['B10'], min: 19000, max: 24000, palette: ['blue', 'red', 'orange', 'yellow']}, 'Landsat8-Thermal', 1)

As always, you can access the complete GEE filtering functionality by
tipping `ee.Filter` into the search bar of the **Docs tab**.

#### Reducing

We have so far selected and printed the first image of the list. Now we
want to create a full mosaic of the study area with images that are free
of clouds. The reduce function allows to reduce image collections to a
single image. It is a way to aggregate data over time
(`imageCollection.reduce()`), space (`image.reduceRegion()`,
`image.reduceNeighborhood()`) or bands (`image.reduce()`).

The `ee.Reducer()` can be a used to operate a simple statistic for
aggregating data (e.g. minimum, maximum, mean, median, standard
deviation, etc.), or for more complicated analysis on the input data
(e.g. histogram, linear regression, list). In all these cases, the
reducer takes an input data set and calculates a single output. When a
single input reducer is applied to a multi-band image, such as Landsat
image, GEE automatically applies the reducer separately to each band,
producing an output image with the same number of bands as the input.

Continuing with our example, this time we will select all images with
less than 10% cloud cover and estimate the median value for each pixel
and each band in the Landsat 8 collection. To retain only images with
&lt;10% cloud cover we will use the function, `filterMetadata()` that
requires three arguments: The name of the metadata property, the name of
a comparison operator (it can be: “equals”, “less\_than”,
“greater\_than”, “not\_equals”, “not\_less\_than”, “not\_greater\_than”,
“starts\_with”, “ends\_with”, “not\_starts\_with”, “not\_ends\_with”,
“contains”, or “not\_contains”) and a number for the cloud cover
percentage to compare against. We will also increase the time period to
include images from the first day of 2015 to the last day of 2019 to
obtain a five years mosaic of our study area.

    var l8_mosaic_1 = ee.ImageCollection("LANDSAT/LC08/C02/T1")
        .filterBounds(Bounds)
        .filterDate('2015-01-01', '2019-12-31')
        .filterMetadata('CLOUD_COVER', 'less_than', 10)
        .reduce(ee.Reducer.median());

    print(l8_mosaic_1)

The reducer is going to change the name of the bands. When printing the
image, you can see that the band names have changed (Fig. 6).

<center>

<figure>
<img src="FigCh2/Figure6.JPG" style="width:60.0%"
alt="Figure 6. Band names for the mosaic after applying a median reducer." />
<figcaption aria-hidden="true">Figure 6. Band names for the mosaic after
applying a median reducer.</figcaption>
</figure>

</center>

Instead of using the reducer, you can use the built-in function
`median()` that will do the same operation on each pixel across the
stack of all matching bands but retain the original band names.

    var l8_mosaic_2 = ee.ImageCollection("LANDSAT/LC08/C02/T1")
        .filterBounds(Bounds)
        .filterDate('2015-01-01', '2019-12-31')
        .filterMetadata('CLOUD_COVER', 'less_than', 10)
        .median();

    Map.addLayer(l8_mosaic_2, {bands: ['B4', 'B3', 'B2'], min: 7000, max: 12800}, 'Landsat8-Mosaic', 1);

As we develop more complex analysis along the tutorial, we will learn more
specifics on the reducer functions. You can look into the `ee.Reduce`
methods that exist in the API.

#### Band math

Another powerful functionality is performing mathematical operations on
images or image bands. For example, you can use a combination of bands
to calculate vegetation indexes. One of the most widely used vegetation
indexes is the Normalized Difference Vegetation Index (NDVI). This index
provides an estimation of vegetation productivity, thus, is widely used
in spatial ecology studies. It is calculated using the near infrared and
red bands as follows:

*N**D**V**I* = (*N**I**F*−*R**E**D*)/(*N**I**R*+*R**E**D*)

To demonstrate the versatility of GEE to perform mathematical operations
on images, we will calculate NDVI using different methods.

Some simple image math can be performed by using `add()`, `subtract()`,
`divide()`, `multiply()`, `pow()`, etc. Those operators can be applied
on numbers, images or arrays. We can use these operators to calculate
NDVI.

First, we need to `select` the desired bands from the image and then
apply the mathematical operation. For our example, we need the red band
(band 4) and the near infrared band (band 5):

    // Method 1 - Applying band operations
    var ndvi_1 = l8_mosaic_2.select('B5').subtract(l8_mosaic_2.select('B4'))
      .divide(l8_mosaic_2.select('B5').add(l8_mosaic_2.select('B4')));

However, it is generally the case that you need to perform more
complicated mathematical operations. For those cases, you can use the
`expression()` function, which allows to represent math operation in
text forms.

    // Method 2 - Applying band opperations using a expression
    var ndvi_2 = l8_mosaic_2.expression('(B5 - B4) / (B5 + B4)', {
        B5: l8_mosaic_2.select('B5'),
        B4: l8_mosaic_2.select('B4')
    });

Note that `expression()` returns an integer if two integers are divided,
so that the expression 10 / 20 = 0. To obtain decimal numbers as
results, you need to have decimal numbers in the operations. For
instance, in the previous case, you have to multiply one of the operands
by 1.0: 10 \* 1.0 / 20 = 0.5.

For many basic operations, such as calculating vegetation indexes, often
exists a built-in function in Earth Engine that makes things much
easier. Here, we will use the `normilizedDifference()` function. We need
to provide the name of the two bands to use.

    // Method 3 - Using a built-in function
    var ndvi_3 = l8_mosaic_2.normalizedDifference(['B5', 'B4'])

Finally, it may be the case that you want to write your own function.
The advantage of writing a function is that you can then apply the
function to any image you want to work with. Here, we are going to write
a function that calculated NDVI (function `normilizedDifference()`),
renames the resulting band as NDVI (function `rename()`) and adds it to
the image as a new band (function `addBands()`).

    // Method 4 - Building your own function to compute NDVI and add it as a new band to the image
    var addNDVI = function(image) {
        var NDVI = image.normalizedDifference(['B5', 'B4']).rename('NDVI')
        return image.addBands(NDVI) //with addBands, the new NDVI is added as a new band to the existing bands.
    };

    // Apply the function to the mosaic
    var l8_mosaic_3 = addNDVI(l8_mosaic_2);

You can also use the function `map()` to apply the addNDVI function you
have created to an entire image collection. As a result, all the images
on the collection will have an additional band called NDVI.

    var l8_NDVI = ee.ImageCollection("LANDSAT/LC08/C02/T1")
        .filterBounds(Bounds)
        .filterDate('2015-01-01', '2019-12-31')
        .filterMetadata('CLOUD_COVER', 'less_than', 10)
        .map(addNDVI);

#### Masking

Another common operation when working with images is masking. Masking
refers to setting certain pixels from an image to no data values (i.e.,
making them transparent) in order to exclude them from analyses. Masking
is usually done to remove pixels with poor data, representing clouds or
any other area that wants to be excluded.

Every pixel in a band of an `ee.Image`, in addition to its value, has a
mask which ranges from 0 (i.e., no data) to 1. In Earth Engine, all
masked pixels (0) are treated as no data. When applying a mask, pixels
with a value of 0 are then excluded from operations. For instance, when
applying `image1.mask(image2)`, the values of image2 are taken and used
as a mask of image 1, meaning that pixels in image2 that have the value
0 will be made transparent in image1.

Continuing with our example, we may be interested in removing all the
ocean water from the NDVI image. We need to create a mask that will
retain only the land pixels (Fig. 7).

<center>

<figure>
<img src="FigCh2/Figure7.JPG" style="width:80.0%"
alt="Figure 7. Masking process." />
<figcaption aria-hidden="true">Figure 7. Masking process.</figcaption>
</figure>

</center>

Using the elevation data, we can use the function `gt()` (grater than)
to create a binary image with a value of 1 for all pixels with
elevations greater than 0, and a value of 0 to pixels with values lower
than 0. Then we use the function `updateMask()` to retain only NDVI
values from the land (i.e., elevation &gt; 0).

    // Create a land mask using SRTM elevation data.
    var watermask =  ee.Image("USGS/SRTMGL1_003").gt(0);

    // Update NDVI mask with the land mask.
    var maskedndvi_1 = ndvi_1.updateMask(watermask);

    // Display the masked result.
    Map.addLayer(maskedndvi_1, {min:0, max:1}, 'NDVI masked');

Masks can be particularly useful in cases where you cannot find images
that are cloud free and you need to mask those pixels for analysis. We
will explore more of these type of operations using the image metadata
on pixel quality to create cloud masks for different satellites in
following chapters.

### Working with features

In addition to the raster datasets, we can use vector data in GEE.
Vector data is handled with the Geometry type. Geometries in Earth
Engine can be **Point** (a list of coordinates in some projection),
**LineString** (a list of points), **LinearRing** (a closed LineString),
and **Polygon** (a list of LinearRings where the first is a shell and
subsequent rings are holes). **MultiPoint**, **MultiLineString**, and
**MultiPolygon** are also supported.

#### Geometries and features

You can create Geometries using the Code Editor geometry tools, as you
did at the beginning of the chapter to define your study area. However,
you can also use code to create geometries, by providing a list of
coordinates. The following code will create the polygon representing the
study area defined earlier. All we need is a list of pair of
coordinates, indicating the vertices of the polygon. Check that the
first and last coordinates are the same.

    var polygon = ee.Geometry.Polygon([
      [[97.58, 9.32], [100.43, 9.32], [100.43, 14.43], [97.58, 14.43], [97.58, 9.32]]
    ]);

Geometries can then be converted into features. A **Feature** is an
object with a geometry property storing a **Geometry** object (or null)
and a **property** storing a dictionary of other properties. In order to
create a feature, you need to provide a geometry, but also a dictionary
with other properties of interest.

    var polyFeature = ee.Feature(polygon, {Area: 'Tanintharyi'});

**Geometry** and **Feature** objects can be printed or added to the map
similarly to images using `Map.addLayer()`. The default visualization
parameters will display vectors with solid black lines and semi-opaque
black fill. You can change the colors similarly to images.

    print(polyFeature);
    Map.addLayer(polyFeature, {}, 'Study area');
    Map.addLayer(polyFeature, {color: 'red'}, 'Study area - red');

#### Feature collections

Similar to image collections, you can create a feature collection, that
is as the name implies, a collection of features. To do this, we need to
apply `ee.FeatureCollection()` on a list of features. For instance, we
can create a list of feature points, each of which represents a town
with its coordinate (the geometry) and a dictionary with the name of the
town. We can then combine all the town into a feature collection.

    // Make a list of Features
    var feature_list = [
      ee.Feature(ee.Geometry.Point(98.62, 12.44), {name: 'Myeik'}),
      ee.Feature(ee.Geometry.Point(98.76, 11.25), {name: 'Bokpyin'}),
      ee.Feature(ee.Geometry.Point(98.52, 13.39), {name: 'Pe Det'})
    ];

    // Create a FeatureCollection from the list, print it and display it on the map
    var Towns = ee.FeatureCollection(feature_list);
    print(Towns);
    Map.addLayer(Towns, {color: 'blue'}, 'Towns');

Google Earth Engine offers several **Feature collections** through the
Data Catalog that can be imported. They are mostly data sets for the USA
but, in addition to the protected areas we used, there are also country
boundaries and ecoregions, among others. This is a much smaller catalog
than the one for image collections, but the amount of data available
grows every day.

#### Filtering feature collections

Feature collection can be large, and similar to image collections, we
may need to filter them. We will also use filters to do this. Here, we
will load the World Database on Protected Areas and filter it to our
study area, similarly to what we did before to filter the Landsat image
collection.

    var PA = ee.FeatureCollection('WCMC/WDPA/current/polygons');

    var PA_filtered = PA.filterBounds(Bounds);

You can now check how many protected areas are within the study area by
using `size()` which returns the number of entries in a dictionary.

    print('Count after filter:', PA_filtered.size());

Now, let’s filter the protected areas on our study area that are Marine
National Parks. We can use the `ee.Filter.eq()` (eq for equal) function
to retain only those protected areas designated as Marine National Park
and check how many there are. As with the `filterMetadata`, you need to
provide the property name of the feature to filter on and the value to
compare against.

    var MarineNatParks = PA_filtered.filter(ee.Filter.eq('DESIG', 'Marine National Park'));

    print('Number of Marine National Parks:', MarineNatParks.size());

### Mapping functions over feature collections

Similar to what we learned about mapping functions over image
collections, we can do it on feature collections. Let’s say you are
interested in calculating the area of each Marine National Park. We will
do this as an exercise even though the Protected Area database already
has a property with the area of each feature.

First, we define a function to apply to each feature. The function
calculates the area using the `area()` function. We then divide this by
1000000 to obtain area in square kilometers. The resulting number is set
to each feature as a new property that we called areakm2.

Second, we map the `addArea` function we created across the
**MarineNatParks** feature collection.

    // Compute the area of each National Park in km2

    // Function to compute patch area and perimeter and add it as a property
    var addArea = function(feature) {
      return feature.set({areakm2: feature.area().divide(1000 * 1000)});
    };

    // Map the area using the function over the FeatureCollection
    var Area = MarineNatParks.map(addArea);

#### Reducing feature collections

We can also apply reducers to feature collections. The idea is the same
than for image collections, aggregate data over the collection. Here, we
use the `reduceColumns()` function on the Area feature collection
previously created. We need to specify the reducer we want to apply, in
this case the `ee.Reducer.mean()`, and the property or list of
properties we want to reduce, in this case `areakm2`. When printing this
object we have a new property with the mean area of all National Parks
in the collection.

    // Calculate the mean area across parks
    print('Mean National Park area (km2):', Area.reduceColumns(ee.Reducer.mean(), ['areakm2']));

#### Displaying fature and feature collections on the map

Finally, you can display the protected areas and Marine National Parks
on the map. We use the same function than more images,
`Map.addLayers()`.

    Map.addLayer(PA_filtered, {color: 'green'}, 'Protected Areas');
    Map.addLayer(MarineNatParks, {color: 'blue'}, 'Marine NP'); 

### Exporting resuts

Lastly, at the end of an analysis you may want to export your results
and data products. There are many reasons you may want to do this. The
most obvious is to create figures and maps using another software, such
as [QGIS](https://qgis.org/en/site/) or to use raster and vector
products in other analysis in software such as
[R](https://www.r-project.org/).

There are different type of products that can be exported from GEE.
These include images, map tiles, features, tables and videos. The data
can be exported to your linked Google Drive account, as a new Earth
Engine asset that will appear on your asset manager, or to Google Cloud
Storage (Note this is a fee-based service). We will not cover Cloud
Storage in this tutorial, but you can learn about Cloud Storage
[here](https://developers.google.com/earth-engine/cloud/earthengine_cloud_project_setup?hl=en)
and how to set projects
[here](https://developers.google.com/earth-engine/cloud/projects?hl=en).

Here, we are going to demonstrate how to export an image and a feature
collection. We will see more about these functions along the different
chapters.

#### Exporting an image

Imagine that you want to create a map of your study area displaying the
NDVI values across the region. You can then export the final NDVI image
we created previously **maskedndvi\_1**. We will export the image to
Google Drive, where it can be downloaded to be used in other software.
For this, we will use the function `Export.image.toDrive()`.

All export functions have a series of arguments that we need to
complete. First, we need to specify the `image` we want to export, in
our case the **maskedndvi\_1**. Second, we need to provide a
`description` that will be the file name when saved in Google Drive.
Then we need a `scale` parameter, the spacial resolution or pixel size
in meters, and a `region`, a ee.Geometry object that defines your area
of interest. In our example, we have been using **Bounds**. Another
important argument is the `maxPixels`. The default is 1e8 pixels, but
you can increase this number as needed. Note that when the image is too
big, it will be exported in tiles as 2 or more images that then you can
combine. The image default output is **GeoTIFF**, but the **TFRecord**
format is also supported.

There are several other optional arguments for this functions. Here we
show two extra arguments. The `folder` within your Google Drive to keep
it organized (By default the image will be saved in the Drive root
directory) and the `crs`. The coordinate reference system is optional,
but we include it here because it is important to keep track of the crs
used when exporting data to use in other GIS software.

Here is the code:

    // Export raster images to Google Drive
    Export.image.toDrive({
      image: maskedndvi_1,  // Export the masked NDVI as an example
      region: Bounds,
      description: 'NDVI-30m',
      folder: 'GIS',
      scale: 30,
      fileFormat: 'GeoTIFF',
      maxPixels: 1e10,
      crs: 'EPSG:4326'
    });

In other cases, you may be interested in saving an image as an asset, so
you can use it later in other analyses. To do this, we will use the
function `Export.image.toAsset()`. The difference with exporting to
Google Drive is that we now need to provide an `assetId` as an extra
parameter and that we do not need to specify a file format anymore.

    // Export raster image to asset
    Export.image.toAsset({
      image: maskedndvi_1,  // Export the masked NDVI as an example
      region: Bounds,
      description: 'NDVI-30m',
      assetId: 'NDVI30mExport',
      scale: 30,
      maxPixels: 1e10,
      crs: 'EPSG:4326'
    });

There are other optional arguments that can be modified. Look into the
Export of the Docs tab to explore other arguments.

### Exporting a feature collection and tables

Similar to images, we can export feature collections or tabular data,
such as a future collection with no geometry, as CSV, ESRI Shapefile
(SHP), GeoJSON, KML, KMZ or TFRecord formats. Again, we can export to
Google Drive using the function `Export.table.toDrive()` or as an asset
`export.table.toAsset()`. The arguments that we need to specify include
the `collection` to export and a `description` with the name for the
resulting file. When exporting to Google Drive we also need to provide
the `fileFormat` and we will also specify a `folder` in Google Drive.
When exporting as an asset, we need to provide the `assetId`.

Here we will export the marine national parks (the object
**MarineNatParks**) as a shapefile and also save it as an asset.

    // Export feature to Google Drive
    Export.table.toDrive({
      collection: MarineNatParks,  // Export the marine parks as an example
      description: 'Marine_Parks_Tanintharyi',
      fileFormat: 'SHP',
      folder: 'GIS'
    });

    // Export feature to asset
    Export.table.toAsset({
      collection: MarineNatParks,  // Export the marine parks as an example
      description: 'Marine_Parks_Tanintharyi',
      assetId: 'MarineParksExport'
    });

    // Export table data
    Export.table.toDrive({
      collection: Area,
      description: 'exportMarineNationalParkArea',
      fileFormat: 'CSV',
      folder: 'GIS',
      selectors: ['areakm2'] //A list of properties to include in the export; either a single string with comma-separated names or a list of strings.
    });

Finally, we will export the Marine National Park area as table data in a
CSV format. This object will not have a geometry associated, only the
area we calculated for each feature. For this we use the
`Export.table.toDrive()` we used before with the difference that now the
`fileFormat` is CSV. We also use the `selectors` argument to specify the
property we want to export, NAME and areakm2 of each park. You can add
other properties if wanted.

    // Export table data
    Export.table.toDrive({
      collection: Area,
      description: 'exportMarineNationalParkArea',
      fileFormat: 'CSV',
      folder: 'GIS',
      selectors: ['NAME','areakm2'] //A list of properties to include in the export; either a single string with comma-separated names or a list of strings.
    });

Note that to complete the export process you need to submit the task by
clicking the `run` button on the Tasks tab on the right panel (Fig. 8).
When the process is completed, the taks changes to a blue color as shown
in Fig. 8.

<center>

<figure>
<img src="FigCh2/Figure8.png" style="width:60.0%"
alt="Figure 8. Exporting images and features." />
<figcaption aria-hidden="true">Figure 8. Exporting images and
features.</figcaption>
</figure>

</center>

You will see the csv file in your Google Drive (Fig. 9).

<center>

<figure>
<img src="FigCh2/Figure9.png" style="width:80.0%"
alt="Figure 9. CSV file exported with name and area of Marine National Parks." />
<figcaption aria-hidden="true">Figure 9. CSV file exported with name and
area of Marine National Parks.</figcaption>
</figure>

</center>

### Conclusion

So far, we have seen some common functions to apply on images and
features. There are an enormous amount of possible operations that can
be applied to filter collections, calculate new bands, etc. As we
progress with different types of analyses, you will get more
familiarized with the different possible operations.

## Land-Cover Land-use classification in Google Earth Engine

In this chapter we will learn how to perform a Land-Cover Land-use
classification using Google Earth Engine.

There are two main types of classification, supervised and unsupervised
classifications. In the unsupervised classification, you let the
algorithm define the land cover classes based on the data. In the
supervised classification you will define the land cover classes and
train the algorithm.

Here, we will work with the supervised classification process.

### Define your study area

We will start with a fresh new script. As before, set up your name, date
and topic of the analysis.

    //////////////////////////////////////////////////
    // My name
    // Date
    // Land cover Land use supervised classification
    //////////////////////////////////////////////////

The next step is to define your area of interest or study area. Remember
you can do this using the drawing tools or you can define a polygon
using code.

    //Specify 'Focal Area' for your impact study
    var StudyArea = ee.Geometry.Polygon([
      [96.20400705077832,23.942516412253614],
      [96.3197926495332,23.942516412253614],
      [96.3197926495332,24.021486550182036],
      [96.20400705077832,24.021486550182036],
      [96.20400705077832,23.942516412253614]
    ]);

Next, center the map into the study area that you just defined.

    Map.centerObject(StudyArea);

### Loading the satellite imagery

The main component of any land cover classification is the satellite
imagery. The information on the different bands of the image is what we
are going to use to classify the different types of land use.

Here, we will work with Landsat 8. At this point, you should be familiar
with some basic functions regarding this data set, such as, loading the
image collection and filtering the image collection.

We will get the image cleaning process to another level of complexity.
Before, we removed all images with clouds to avoid having clouds that
will affect the classification process (You want to have a clear image
of the land surface.). This time, we are going to use the metadata of
the Landsat image to mask all the pixels that contain clouds or have a
bad quality.

> Note that we are working with Landsat 8. There are other Landsat
> products that require some modifications to the filters. We will
> examine that later on when classifying images from the past to access
> land cover change.

What we need to do first is create a function that will mask all pixels
that correspond to clouds or cloud shadows. The information is contained
in a band called **QA_PIXEL**.

This function first creates a mask converting all pixels from the
*QA_PIXEL* band of the Landsat image (Fig. 1) that correspond to cloud
and cloud shadows
`image.select(['QA_PIXEL']).bitwiseAnd(Math.pow(2,3)).` into zeros
`eq(0)`, the mask. It then applies the mask to all the bands on that
image.

<center>

<figure>
<img src="FigCh3/Figure1.png" style="width:90.0%"
alt="Figure 1. Landsat 8 Surface Reflectance image." />
<figcaption aria-hidden="true">Figure 1. Landsat 8 Surface Reflectance
image collection.</figcaption>
</figure>

</center>

The expression `Math.pow(2,3)` obtains the bit value that identifies the
pixel with cloud of cloud shadow. You could do the same for snow, water,
etc.

    // Function to mask clouds based on QA values from the Landsat Surface Reflectance Code (LaSRC) - for Landsat 8
    var lasrcMask = function (image) {
        var mask = image.select(['QA_PIXEL']).bitwiseAnd(Math.pow(2,3)).eq(0).and(  // Cloud shadow
                   image.select(['QA_PIXEL']).bitwiseAnd(Math.pow(2,5)).eq(0));  // Cloud
        return image.updateMask(mask);
    };

We are also going to create a function to `select()` and `rename()`
certain bands from the Landsat image. This may not be really useful if
you are working with only one Landsat product, but because different
Landsat satellites have different bands, it became important for more
advanced analysis that needs to integrate images from different
satellites (Landsat 4 and Landsat 8 for instance.).

    // Function to rename Landsat 8 bands for cross-Landsat compatibility & rescale
    var renameBandsL8 = function(image) {
        var imgNewBands = image.select(['SR_B2', 'SR_B3', 'SR_B4', 'SR_B5', 'SR_B6']).rename(['blue', 'green', 'red', 'nir', 'swir1']);
        return imgNewBands.copyProperties(image,['system:time_start']);
    };

Finally, we will create a function to compute NDVI. This one should be
more familiar. Note that we want to add the new NDVI band to the rest of
the bands in each image, thus, we add `add.Bands(ndvi)` to the return of
the function.

    // Function to compute NDVI
    var addNDVI = function(image) {
        var ndvi = image.normalizedDifference(['nir', 'red']).rename('ndvi');
        return image.addBands(ndvi);
    };

Now that we have all the functions defined, we can load the Landsat 8
Surface Reflectance collection.

We will apply a couple of filters. First `filterBounds()` to keep images
that intersect the study area. We will also retain images that have less
than 20% of cloud coverage. Finally, we will work with images from 2019
to create a year composite.

    // Load Landsat surface reflectance images from 2016-2018
    var l8sr = ee.ImageCollection('LANDSAT/LC08/C02/T1_L2')
                    .filterBounds(StudyArea)
                    .filterMetadata('CLOUD_COVER', 'less_than', 20)
                    .filterDate('2019-01-01', '2019-12-31');
    print(l8sr);

If you print the image collection to the console, you will see that you
have retained 15 images:

<center>

<figure>
<img src="FigCh3/Figure2.png" style="width:90.0%"
alt="Figure 2. Landsat 8 Surface Reflectance filtered images." />
<figcaption aria-hidden="true">Figure 2. Landsat 8 Surface Reflectance
filtered images.</figcaption>
</figure>

</center>

We now can apply the functions to mask cloud pixels, rename bands and
add NDVI by using the function `map()`.

Finally, we will create a median composite by calculating the `median()`
value for each pixel in each band across all available images and
`clip()` the final image to the study area.

    // Apply functions
    var l8sr8nocld = l8sr.map(lasrcMask)
                          .map(renameBandsL8)
                          .map(addNDVI);
                        
      
    // Create a median composite image (takes the median value from each band across all available images)
    var l8srcomp = l8sr8nocld.median().clip(StudyArea);

You can now display some band combinations into the map. We will use the
final composite together with the high resolution image provided by
Google to create training data.

    // Display image
    Map.addLayer(l8srcomp, {bands: ['red', 'green', 'blue'], gamma: 1, max: 14139, min: 8505, opacity: 1}, 'L8 SR True color');
    Map.addLayer(l8srcomp, {bands: ['nir', 'red', 'green'], gamma: 1, max: 18376, min: 9196, opacity: 1}, 'L8 SR False color');
    Map.addLayer(l8srcomp, {bands: ['ndvi'], palette: ['white','green'], max: 1, min: 0, opacity: 1}, 'NDVI');

### Idenifying land cover classes

The next step in the supervised classification process is to define the
classes that you will work with. This is an important step. Complete….

For this example, we will work with four classes: 1. Forest 2. Water 3.
Sand 4. Agriculture

We need to train a model to identify these four land cover classes
across the image composite. Thus, we need to indicate to the algorithm
where those areas are. For this, we will use the drawing tools to draw
polygons on top of the Landsat image composite where the land cover is
distinctive. You can use the high-resolution image provided by Google to
help you find clear areas. If your polygons are not well defined, then
the algorithm will confuse classes.

> Note that some areas are highly dynamic, such as the sand banks along
> the river. Make sure your polygons match the Landsat image, as this is
> the image you are using to train the algorithm.

You need to create a new geometry for each class. Then you can draw as
many polygons as you want for each class. Start with at least 10
polygons for each class. Try to account for all possible spectral
variation across the class.

Choose a proper color to represent each class.

Here is a demonstration on how to crate and name the different
geometries for the four classes.

<center>

<img src="FigCh3/TrainingData.gif" style="width:90.0%" />

</center>

Use the hand tool to select and edit any polygons or delete them.

### Creating training and validation data sets

The next step is to create random points in each polygon to extract the
spectral information on those pixels. This is going to be the data set
to train and validate your classification model.

The first step is to combine all geometries using the `merge()` funtion.

    // Merge training polygons for land cover map
    var LandCovers = [Forest,Agriculture,Sand,Water];

The next steps are going to be more challenging. Take your time to
understand every step in the code.

Now that we have all geometries combined, we need to convert them into a
feature and add a numeric class property. Instead of doing this one by
one, we can create a loop.

In a loop we start with the command `for()`. The `for()` takes 3
arguments. The first one is the variable to loop over and the value at
which to start (We call this variable i and start at 0). The second
argument is where to end the loop. Here instead of hard coding 3
(Remember that in java script the first element is in position 0, so the
fourth element is in position 3), we will use the function
`LandCovers.length` to get the number of classes and use the symbol `<`
to get one minus the total number of classes. The advantage of using
code is that you can later add more classes, and the loop will still
work as it will automatically adjust for the new classes. The final
argument increases the value each time the code in the loop is executed.

    // Convert training data geometries to features and add numeric 'Class' property
    for (var i = 0; i < LandCovers.length; i++){
        LandCovers[i] = ee.Feature(LandCovers[i]).set('Class', i);
    }

> Remember that we need to use the ee.Thing notation.

It is ideal to have ground independent data to validate your model
predictions. However, many times it is not possible to gather such
information. One solution is to split your data set into a percentage
for training and the rest for validating. We will use 70% for training
and 30% for validation.

We also need to define the number of random points that we want to
sample per class. A rule of thumb is to have for *n* bands of data, at
least 10<sup>*n*</sup> pixels for each class. For this example we are
going to work with 6 bands (blue, green, red, nir, swir1, ndvi) and 4
classes.

    // SAMPLE TRAINING AND VALIDATION PIXELS FROM EACH CLASS
    var split = 0.7;   // What proportion of your data are used to train the model?
    var N = 500;       // Define the number of pixels for each class to be randomly sampled from training polygons

Now we need to create random points for each class to extract the pixel
values and split the data set into training and validation as previously
defined.

To do this, we will have to loop over all polygons created.

We first need to define two functions that we will need inside the loop.
One function to transform each point into a feature and add a property
class. And a second one to reassign the class after processing.

    var addClass = function(poly){
      return ee.Feature(ee.Geometry(poly)).set('Class',i);   // convert to feature and set Class
    };
      
    var ptsClass = function(f) {
      // Set class value for sampled points
        return f.set({
          Class: thisClass
        });
    };

Now define two empty variables to store the training and validation data
sets.

    // Define empty variables
    var trainVals;
    var testVals;

Now is the big loop. Here is the entire code, and then we will walk
trough line by line

    for (var i = 0; i < LandCovers.length; i++){
      var geometries = LandCovers[i].geometry().geometries();   // extract individual geometries
      var extractpolys = ee.FeatureCollection(geometries.map(addClass));
      var extractpolys = extractpolys.randomColumn();  // add random number to each feature as property
        
      // Split into training/testing based on random number property
      var trainingPartition = extractpolys.filter(ee.Filter.lt('random', split));
      var testingPartition = extractpolys.filter(ee.Filter.gte('random', split));   
        
      // Sample random pixels from both sets of polygons
      var trainpts = ee.FeatureCollection.randomPoints(trainingPartition.geometry(), (N*split));
      var testpts = ee.FeatureCollection.randomPoints(testingPartition.geometry(), (N-N*split));
        
      // Extract values of current class from training data
      var thisClass = ee.Feature(LandCovers[i]).get('Class');
      
      // Create a function to the class property
      trainpts = trainpts.map(ptsClass);
      testpts = testpts.map(ptsClass);
      
      // Extract pixel values from composite at sampled points
      var trainPixelVals = l8srcomp.sampleRegions(trainpts, ['Class', 'label'], 30);
      var testPixelVals = l8srcomp.sampleRegions(testpts, ['Class', 'label'], 30);
      
      // Combine points across classes
      if (i === 0) {
         trainVals = trainPixelVals;
         testVals = testPixelVals;
      } else {
        trainVals = trainVals.merge(trainPixelVals);
        testVals = testVals.merge(testPixelVals);
      } 
    }

Let’s look at each line:

We first define the arguments for the loop as before, to loop over each
land cover category:

`for (var i = 0; i < LandCovers.length; i++){`

The second line extracts the individual geometry of each polygon for the
first class `i` of the loop:

`var geometries = LandCovers[i].geometry().geometries();`

The third line applies the function `addClass()` to each individual
geometry:

`var extractpolys = ee.FeatureCollection(geometries.map(addClass));`

And the next line adds a random number as a new property to the feature.
We will use this random number to split the data set into training and
validation:

`extractpolys = extractpolys.randomColumn();`

Using the filter `ee.Filter.lt()` (less than) and `ee.Filter.gte()`
(greater or equal than) we split the polygons into training and
validation using the random number we created in the previous line.

`var trainingPartition = extractpolys.filter(ee.Filter.lt('random', split));`

`var testingPartition = extractpolys.filter(ee.Filter.gte('random', split));`

Now we will create random points inside those polygons, first the ones
for training and then the ones for testing. For this, we use the
function `randomPoints()` that takes the polygon geometries as the first
argument and the number of points to create as the second. We will
divide the number of points we previously specified into 70 and 30%.

`var trainpts = ee.FeatureCollection.randomPoints(trainingPartition.geometry(), (N*split));`

`var testpts = ee.FeatureCollection.randomPoints(testingPartition.geometry(), (N-N*split));`

The created random points do not have the class assigned. The next three
lines add the class to the point depending on which polygon they come
from that corresponds to the cycle of the loop. Remember that in the lop
we first perform all these functions to class 1 and then 2 and so on.

`var thisClass = ee.Feature(LandCovers[i]).get('Class');`

`trainpts = trainpts.map(ptsClass);`

`testpts = testpts.map(ptsClass);`

So far we just have created the random points for validation and testing
on the class. Now the important part, get the pixel values from the
image composite. For this, we use the function `sampleRegions`. The
resulting variable will be the points with all the pixel values for each
band and the corresponding class as properties. The number 30 is the
spatial resolution of the Landsat image.

`var trainPixelVals = l8srcomp.sampleRegions(trainpts, ['Class'], 30);`

`var testPixelVals = l8srcomp.sampleRegions(testpts, ['Class'], 30);`

Finally, we have a conditional statement in order to combine all the
points created for the class. If the loop is going trough the first
class, then it is saved into the previously created variables (trainVals
or testVals). If the loop is going trough the second, third, etc. cycle,
then the points are merged to the variables. This is needed because you
cannot merge points into an empty variable.

`if (i === 0) {      trainVals = trainPixelVals;      testVals = testPixelVals;   } else {     trainVals = trainVals.merge(trainPixelVals);     testVals = testVals.merge(testPixelVals); }  }`

Now, lets print and inspect the trainVals variable.

    print(trainVals)

### Applying the classifier

Now that we have the training and validation data sets, we can move to
the next step, training the classifier algorithm. There are many
different type of classifiers. You can look at them typing ee.Classifier
into the search bar of the Docs tab.

For this tutorial, we are going to use the random forest classifier.

The first thing we need to do is to define the bands that we are going
to use for the classification.

    // Indicate input bands for classifier
    var bands = ['blue', 'green', 'red', 'nir', 'swir1', 'ndvi']; 

Now we will apply the random forest classifier using the function
`ee.Classifier.smileRandomForest()`. The argument is the number of
decision trees to create. We will use 100 for fast computation, but you
should use at least 500. The `train()` function specifies the collection
of features to rain the classifier, using the specified numeric
properties of each feature as training data.

    // Classify using Random Forest
    var rfClassifier = ee.Classifier.smileRandomForest(100).train(trainVals, 'Class', bands);

Once we have trained the classifier, we can classify the image composite
(l8srcomp) using the `classify()` function and providing the trained
classifier as the argument. Note that we only select the bands from the
image that we used to train the classifier.

    var rfClassified = l8srcomp.select(bands).classify(rfClassifier);

And now you have a classified image.

The final step is to display the classification on the map.

Use a pallet with colors that are representative of the land cover
classes.

    //Generate pallet   
    var colors = ['darkgreen','orange','yellow','darkblue']

    // Add classified image to the map
    Map.addLayer(rfClassified.clip(StudyArea), {
      palette: colors, 
      min: 0, 
      max: (LandCovers.length-1) // #classes-1
    }, 'Land Cover Map');

### Model validation

The last task in a classification is to test the accuracy of the model.
Thus, how well the classifier actually classified the image. If the
classification is not really accurate, you need to modify the training
data and repeat the process until you get an accuracy value that
satisfies your work.

The first step is a familiar one now, use the `classify()` function
again, but this time on the 30% validation data set.

Next, you will apply the `errorMatrix()` function that computes an error
matrix by comparing the actual values of the training data with those
predicted by the classifier. You need to specify in the arguments the
name of the property containing the actual value and the name of the
property containing the predicted value. You can print the error matrix
in the console.

    // Check the accuracy of your classification
    var rfTest = testVals.classify(rfClassifier);
    var rfAccuracy = rfTest.errorMatrix('Class', 'classification');
    print('RF Error Matrix: ', rfAccuracy);

You can use the confusion matrix to estimate the accuracy of each band,
and with that, see for which classes the model is performing better or
worse. Here, we will use the function `accuracy()` to get the overall
accuracy of the classification, defined as correct classified pixels
over the total.

    var frOvAccuracy = rfAccuracy.accuracy().getInfo();
    print('RF Overall Accuracy: ', frOvAccuracy);

## Species Distribution Models

The tutorial for SDMs can be found [here](https://ramirodcrego.com/teaching/gee/).
