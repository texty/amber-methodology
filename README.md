## Search for illegal amber mining HOWTO

This is a description of methodology for this project: http://texty.org.ua/d/2018/amber_eng.
Main idea - use machine learning to find all places of illegal amber mining in Nothern-West regions of Ukraine, on satellite images.


## Motivation

In 2010 world prices for amber started to surge. Due to this in 2012 demand was so high that north-western part of Ukraine became place of "amber rush" and "new Wild West". Thousands of prospectors starts to search for gems with shovels and later with water pumps. Hundreds of hectars in forests / agricultural land became a desert, a lifeless moon landscape. 2014-2016 were most intence years of illegal mining, but this activity still exists 

We decided to estimate, for the first time, an environmental impact of this phenomenon. 


## Main steps 
(there will be links to python notebooks for some steps)

1. Research how patches of land with illegal mining could look on satellite images (look at images for some known locations with amber mining, use known videos and photos)
2. Find which map providers have relatively recent satellite images with good resolution. Find examples of places with mining on such images.
3. Find and download initial set of images with traces of mining
4. Split each image to superpixels (approx same regions)
5. Use neural net to extract features for each superpixel
6. Create labelled set of superpixels for binary classificator
7. Create machine model to classify superpixels
8. Apply steps 4,5 and classifier to each superpixel for all images from region of interest. If more then 2 superpixels classified as positive, mark image as area of mining.
9. Create interactive map with places found by our method   


Result: we present most informative, as for this moment, interactive map of impact on environment due to illegal amber mining in Ukraine: http://texty.org.ua/d/2018/amber_eng
