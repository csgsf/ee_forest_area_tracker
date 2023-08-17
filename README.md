# OSINT_forest_area_tracker


- The Basics - how to use the tool
- What is a "Before and After" collection?
- What are dNBR classes?
- Using Sentinel-2 to track burned areas
- How do we remove bodies of water from the map?
- How do we account for cloudy images?
- How accurate is this map and calculation?
- How were the predefined regions chosen?
- Fire data
- Resources
  
<img width="1660" alt="image_1" src="https://github.com/csgsf/ee_burn_tracker/assets/90655137/25c12985-446c-493b-b4cb-bb59345a070a">




This documentation relates to the OS tool - OSINT Forest Area Tracker. The tool has been created for researchers, investigators, and journalists to study areas that have been impacted by fires or the destruction of lands. 

While true color imagery (images of buildings or forests in their natural color) is widely used in open-source investigations, there are opportunities to use remote sensing data for more detailed investigations. 

With Google Earth Engine, investigators can make use of satellites' remote sensing data to study specific characteristics, such as vegetation, temperature, and changes in the built environment. 

The OSINT Forest Area Tracker uses specific parameters that reveal the extent of burned areas. The tool was initially created to study the impact of Russia's invasion on Ukraine and its environmental impact, but it can be applied to other forest areas around the world (you just have to know the coordinates!).

###

Areas codes from dropdown tool

01 - Chernobyl Radition & Environmental Biosphere Reserve
02 - Danube Biosphere Reserve
03 - Ivory Coast of Sviatoslav National Park
04 - Chernobyl Special Reserve 
05 - Black Sea Biosphere Reserve
06 - Mizhrichynskyi Regional Landscape Park
07 - Podilsky Tovtry National Park
08 - Korsunskyi Zoological Reserve
09 - Drevlyansky Reserve
10 - Nizhnyodniprovskyi National Nature Park
11 - Kreminski Kaptazhi Hydrological Reserve
12 - Drevlianskyi Natural Reserve
13 - Savynska Lisova Dacha Reserve
14 - Rivne Nature Reserve
15 - Kinburn Spit Regional Landscape Park
16 - Seymskyi Regional Landscape Park
17 - Svyati Hory National Park


### The Basics – how to use the tool

To use this tool, input a range of dates that you would like to collect that will act as one composite before collection. Then, choose a range of dates to compare with. 

For example, perhaps I would like to compare July 2020 with August 2020. I would put: 2020-07-01 & 2020-07-31; 2020-08-01 & 2020-08-31. This will compare the change that has occurred on land between these two time frames. 

Choose a location either by entering coordinates, clicking on your chosen location on the map (which fills the coordinates box), or selecting a pre-determined region in Ukraine. 

Click "Analyze"!

### What is a "Before" and "After" collection?

Before and after satellite images are now a common feature of open-source journalism. I used this technique for an article on the BBC to analyze the destruction of an [ammunitions depot in Equatorial Guinea](https://www.bbc.com/news/world-africa-56337856). 

When using remote sensing data, we can utilize this "before and after" method even more. 

Imagine you want to analyze the impact of a fire in a forest. You could take a snapshot on one day before and after the fire – or you could collect images for the whole month before the fire, then reduce those images into one and average out the presence of trees and compare this with a month after the blaze. This is a more precise method. 

This is the method deployed in the OSINT Forest Area Tracker. For your best chance of success, choose a date range that will include a number of images for the most precise measurement. 

### What are dNBR classes?

dNBR stands for Delta Normalized Burn Ratio. 

For the creation of this application, I drew significantly on the [UN's best practices](https://www.un-spider.org/advisory-support/recommended-practices/recommended-practice-burn-severity/burn-severity-earth-engine) for studying burned areas.

It states: "The Normalized Burn Ratio (NBR) is used, as it was designed to highlight burned areas and estimate burn severity. It uses near-infrared (NIR) and shortwave-infrared (SWIR) wavelengths. Healthy vegetation before the fire has very high NIR reflectance and a low SWIR response. In contrast, recently burned areas have a low reflectance in the NIR and high reflectance in the SWIR band."

Delta simply means difference. So, the difference in NBR between the before and after collections.

### Using Sentinel-2 to track burned areas

The application imports data from [Sentinel-2](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2), a collection available on Google Earth Engine. 

Using Sentinel-2 imagery, it's possible to select [bands](https://gisgeography.com/sentinel-2-bands-combinations/) that allow researchers to focus on particular changes on land. This application selects bands that can account for losses in vegetation and tree canopy. 

A satellite image features multiple bands. Each band represents a range of wavelengths that the satellite sensor is capable of detecting.

Sentinel-2 data collection began 23-06-2015 and is collected around every five days, depending on the location on Earth. 

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

### How can we just analyze forests?

I also used the Dynamic World dataset to build the tool so that it only produced results over areas that have been detected as trees by Dynamic World. The Dynamic World dataset uses machine learning approaches to calculate the probability of different land uses. In this app, I reduced the Dynamic World datasets and retrieve the pixels where the tree datapoint was most common. https://dynamicworld.app/  

### How do we account for cloudy images?

Clouds in images can create problems when collecting remote-sensing data. The solution, as implemented in this tool, is to mask out pixels that feature clouds. The hope is you collect enough images that you get the fullest dataset despite the presence of clouds. 

Sentinel-2 has bands that can detect clouds. 

### How accurate is this map and calculation?

This application is to assist with the investigation and discovery of damaged forest areas. The accuracy of the calculations can be affected by the amount of images collected and the presence of clouds. There are tons of great Earth scientists out there I would reach out to if you want to use these methods for news publications or academic papers. 

### How were the predefined regions chosen?

This tool was initially created to study environmental areas in Ukraine. The areas included are designated by the Ukrainian government as environmentally protected areas. The examples chosen to feature in the tool featured the highest frequency of Modis fires in the first year of Russia's full-scale invasion of Ukraine. 

### Fire data

This tool has a second layer that shows Modis fire data. Toggle this layer on, and it will show the fires that have been detected during time from the start of the collection to the end of the "after" collection. It doesn't get a before and after count - but shows all fires during that time. 

Fires that are present during a time period might not relate directly to an affected area - as burned areas as detected in the aftermath. 


<img width="1667" alt="Screenshot 2023-08-16 at 2 09 08 PM" src="https://github.com/csgsf/ee_burn_tracker/assets/90655137/320e2cb8-7845-4d60-9071-210893390bf4">

### Resources

When using the tool, I would suggest corresponding incidences that have been mapped with other organizations to see how extensive the damage is. 

The [Eyes on Russia](https://eyesonrussia.org/) map can be used for identifying environmental damage in Ukraine. 

[Greenpeace](https://maps.greenpeace.org/maps/gpcee/ukraine_damage_2022/) also has a map that includes incidences in Ukraine. 

The [Bellingcat](https://ukraine.bellingcat.com/) map includes incidences of civilian harm. 


