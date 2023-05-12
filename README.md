# ee_burn_tracker

<img width="800" alt="Screenshot 2023-05-11 at 3 48 17 PM" src="https://github.com/csgsf/ee_burn_tracker/assets/90655137/bdf2bba9-e554-4faf-8182-bde0a1c39f3c">



This documentation relates to the OS tool - OSINT Burned Area Tracker. The tool has been created for researchers, investigators, and journalists to study areas which have been impacted by fires or the destruction of lands. 

While true color imagery (images of buildings or forests in their natural color) are widely used in open-source investigations, there are opportunities to use remote sensing data for more detailed investigations. 

With Google Earth Engine, investigators can make use of satellites' remote sensing data to study specific characteristic, such as vegatation, temperature, and changes in the built environment. 

The OSINT Burned Area Tracker uses specific parameters that reveal the extent of burned areas. The tool was initially created to study the impact of Russia's invasion on Ukraine and its environmental impact, but it can be applied to any region around the world (you just have to know the coordinates!).


### The Basics – how to use the tool

To use this tool, input a range of dates that you would like to collect that will act as one composite before collection. Then, choose a range of dates to compare with. 

For example, perhaps I would like to compare July, 2020 with August 2020. I would put: 2020-07-01 & 2020-07-31; 2020-08-01 & 2020-08-31. This will compare the change that has occured on land between these two time frames. 

Choose a location either by entering coordinates, clicking on your chosen location on the map (which fills the coordinates box), or select a pre-determined region in Ukraine. 

Click "Analyze"!

### What is a "Before" and "After" collection?

Before and after satellite images are now a common feature of open-sources journalism. I used this technique for an article on the BBC to analyse the destruction of an [ammunitions depot in Equatorial Guinea](https://www.bbc.com/news/world-africa-56337856). 

When using remote sensing data, we can utilize this "before and after" method even more. 

Imagine you want to analyse the impact of a fire in a forest. You could take a snapshot on one day before and after the fire – or you could collected images for the whole month before the fire, then reduce those images into one and average out the presence of trees and compare this with a month after the blaze. This is a much more precise method. 

This is the method deployed in the OSINT Burned Area Tracker. For your best chance of success, choose a date range that will include a number of images for the most precise measurement. 


### Using Sentinel-2 to track burned areas

The application imports data from [Sentinel-2](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2), a collection available on Google Earth Engine. 
Using Sentinel-2 imagery, it's possible to selects [bands](https://gisgeography.com/sentinel-2-bands-combinations/) that allow researchers to focus on particular changes on land. This application selects bands which can account for losses in vegetation and tree canopy. 

A satellite image features multiple bands. Each band represents a range of wavelengths that the satellite sensor is capable of detecting.

Sentinel-2 data collections began 23-06-2015, and is collected around every five days, depending on the location on Earth. 

It might be useful to know how many images have been collected in your area of interest. 

You can copy and paste the code below into the [Google Earth Engine code editor](https://code.earthengine.google.com/) and it will print how many times images in a given time period and across and an area were captured. If you haven't already, it's possible to sign up for a Google Earth Engine developer account. 

```
// Define the region of interest
var roi = geometry;// Define the region of interest

// Define the time range of interest
var startDate = ee.Date('2019-01-01');
var endDate = ee.Date('2022-05-11');

// Load the Sentinel-2 image collection
var s2 = ee.ImageCollection('COPERNICUS/S2_SR')
        .filterDate(startDate, endDate)
        .filterBounds(roi);

// Calculate the number of images
var count = s2.size();

// Print the number of images
print('Number of Sentinel-2 images:', count);
```


### How do we remove bodies of water from the map?

For this application, I have eliminated water bodies from the analysis. These areas are not relevant for the study of burned areas. 

I have done this by layering over the [Hansen Forests dataset](https://developers.google.com/earth-engine/datasets/catalog/UMD_hansen_global_forest_change_2021_v1_9#bands), which includes a band that eliminates water areas. 

When you choose an area of interest, you will see that bodies of water are automatically excluded from the analysis. 

### How do we account for cloud images?

### How accurate is this map and calculation?

### How were the predefined regions chosen?

### Fire data

### Other features 



