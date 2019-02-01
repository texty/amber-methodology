---
title: "Leprosy of the Land: using machine learning to search for places with illegal amber mining on satellite images"
date: "September 2018"
author: "Anatoly Bondarenko, Texty"
---

# Abstract
Increased world prices for amber led to illegal mining for this gemstone. There are close-to-surface deposits in Nothern-West part of Ukraine. The total area of region with amber is 70,000 km^2 and could be covered by 450,000 satellite images. We developed XGBoost and ResNet based classifier, trained it with initial ground-truth images and created most complete interactive map of this phenomena [1]

# 1. Background

Around 2010 world prices for amber started to surge. Due to this in 2012 demand was so high that north-western part of Ukraine became place of "amber rush" and "new Wild West". Thousands of prospectors starts to search for gems with shovels and later with water pumps. Hundreds of hectars in forests / agricultural land became a desert, a lifeless moon landscape. 2014-2016 were most intence years of illegal mining, but it's still going till now. 

We decided to estimate, for the first time, the scale of environmental impact from this phenomenon and to find places most suffering from ecological, social and criminal consequences from such an activity.   


# 2. Project stages 

First of all, we researched how traces of illegal mining could look on satellite images. For this we compiled some known locations from previous reporting about this topic, used known videos and photos, and made interviews with field experts. Basically, it's just a pits and holes in the ground with white and sand colored features. [pic 1]

Next we found which known map providers have relatively recent satellite images with good resolution. At the end we choosed [3] by Microsoft, for good API with big limit for use of images and date when each specific photo were shot. Due to small characteristic size of digged holes, we needed resolution no less then 1m per pixel. 

Time distribution of satellite images, by year:

| Year | Number|
| ------------- |:-------------:|
| 2011      | 1933|
| 2012      | 4669      |
| 2014 | 117059      |
| 2015 | 271893      |
| 2016 | 55403      |

  
On the next step we manually found and compiled initial set of coordinates for images with traces of mining. First such places were detected with a huge help from participants of Open Data Day Kyiv, in March 2018. 

We splitted each image(tile) to superpixels, or segments with approximately homogeneous visual appearance, with a simple linear iterative clustering [3]. Next we programmed one-click labeller for images (single page web-app in javascript) and labeled each segment as one of the two classes: "with amber minig" and "without mining". In such a way for a couple of hours we obtained initial training set of about 1000 such segments.  

We used transfer learning approach and extracted features for each segment --- it's a vector of dimension 2048 for each image --- with a help of pretrained, vanilla ResNet50 [4] deep learning model from Keras library [5]. 

![Positive examples](positive_types_combined.png)

Next we created machine model to classify superpixels. XGBoost classifier [6] was choosed due to best performance, after several attempts (we tried SVM as a baseline model and RandomForest). Performance of the production model: 

|metrics|result|
|f1|0.91| 
|recall|0.88|
|precision|0.95| 


# 3. Visual examination of the model's performance

Initial training set later was augmented with a help of simple visual method. Specifically, we made dimencionality reduction of ResNet features for images by UMAP method [5], and made a jupiter notebook with interactive scatterplot: dots as 2d-projection of each image from validation set, and tooltips - as a corresponding images for each dot. Points on the chart colored by four categories, corresponding to entries of confusion matrix. So one could immediately spot colored outlier, look at image for this dot, look at neighbors and augment training set with new examples. This visual method led to biggest increasing in performance of the model.     

![illustration scatterplot](visual debugging)

After that we used our model to classify each superpixel/segment for all images from region of interest. If classifier detected more then 2 superpixels as positive for amber mining, our script marked current tile to place it on map. We processed approximately 450,000 images, with 1000x1000 size, from region with total area about 70,000 km^2. Total computation time was about 100 hours on one computer with two GeForce GTX 960 graphic cards.

!(debug the tiles)[map_prefinal_debug.jpg]

# Results 

We created most detailed, as for this moment, interactive [map of impact on environment due to illegal amber mining in Ukraine](http://texty.org.ua/d/2018/amber_eng) (this article is in English). This approach could be useful for detection other types of interesting features online satellite images. For example, it could be used to estimate scale of melting of permafrost in Arctic region. For reproducibility and future use by others, we published detailed methodology[]

# Acknowledgements


# References
[1] [Leprosy of the Land](http://texty.org.ua/d/2018/amber_eng)
[2] [Bing Maps](https://www.bing.com/maps?osid=6c00a44b-a9e3-4162-9c6d-6a962b7a717e&cp=50.528222~28.304432&lvl=15&style=h&v=2&sV=2&form=S00027)
[3] [SLIC algorithm](http://www.kev-smith.com/papers/SLIC_Superpixels.pdf)

[4] ResNet
Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun 
Deep Residual Learning for Image Recognition
arXiv:1512.03385, 2015

[5] [Keras library](https://keras.io/) 
François Chollet and others, Keras Library, https://github.com/fchollet/keras, 2015

[6] UMAP method 




