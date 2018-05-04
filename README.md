## Search for illegal amber mining HOWTO

This is a description of methodology for this project: http://texty.org.ua/d/2018/amber_eng.
Main idea - use machine learning to find all places of illegal amber mining in Nothern-West regions of Ukraine, on satellite images.


### Motivation

In 2010 world prices for amber started to surge. Due to this in 2012 demand was so high that north-western part of Ukraine became place of "amber rush" and "new Wild West". Thousands of prospectors starts to search for gems with shovels and later with water pumps. Hundreds of hectars in forests / agricultural land became a desert, a lifeless moon landscape. 2014-2016 were most intence years of illegal mining, but this activity still exists 

We decided to estimate, for the first time, an environmental impact of this phenomenon. 




### Project stages 

1. Research how patches of land with illegal mining could look on satellite images (look at images for some known locations with amber mining, use known videos and photos, interviews with field experts)
2. Find which map providers have relatively recent satellite images with good resolution. Find examples of places with mining on such images (Mostly it's Bing. It has very good API with metadata for each tile.)
3. Find and compile initial set of coordinates for images with traces of mining
4. Split each such tile to superpixels/segments (regions with approximately homogeneous visual appearance)
5. Use neural net to extract features for each superpixel (was used vanilla ResNet50 from Keras library )
6. Create labelled set of superpixels for binary classificator (split images on two sets - with traces of amber mining, and without such traces)
7. Create machine model to classify superpixels (XGBoost was choosed after several tries)
8. Apply steps 4,5 and classifier from 7 to each superpixel/segment for all images from region of interest. If more then 2 superpixels classified as positive, mark current image as area of mining. (approximately 450,000 images for region with total area about 70,000 km^2, total computation time was ~100 hours on computer with two GeForce GTX 960 onboard)
9. Create interactive map with places found by our method ( Most active period of mining was during 2014-2016 years. Most tiles from maps dated by 2015. We found more then 1,000 hectares of damaged land)


Result: we present most informative, as for this moment, interactive [map of impact on environment due to illegal amber mining in Ukraine](http://texty.org.ua/d/2018/amber_eng) (in English)


### Python notebooks

* [Step 1: Split map tile to "superpixels"](./model/step1.ipynb)
* [Step 2. Extraction of features. Model training/testing](./model/step2.ipynb)
* [Step 3: Detect places with amber mining](./model/step3.ipynb)



### Sources of inspiration

We first thought about amber related project back in 2016, during work on [deforestation in Karpathian mountains](http://texty.org.ua/d/deforestation/). But in that time we didn't have relatively recent satellite images with big enough resolution. 

I'd like to say couple of kind words for (Terrapattern)[http://www.terrapattern.com/] , amazing project which prove that similar analysis of satellite images is possible.


### Contact autor of this repository:

[Anatoliy Bondarenko](https://twitter.com/dvrnd)