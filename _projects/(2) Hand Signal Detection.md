---
name: Hand Signal Detection
tools: [Unity, Data Networking, Neuropype]
image: 
description: 
---

# Hand Signal Detection
## Project Overview
During my first year at Olin, I joined the Human Augmentation Laboratory, which is investigating how Augmented Reality can be integrated into daily life to optimize daily life. Towards this goal, I was tasked with developing a proof-of-concept system that can analyze a user's hand signals and trigger a corresponding response on our MagicLeap (INSERT LINK HERE), an Augmented Reality Headset. The project is split into two main phases: backend training and real-time classification.

## Backend Training
In order to classify hand signals in real time, we needed to develop a large dataset and train a backend machine learning model. This was done by extracting electromyography data from the hands of three different individuals using a Cyton Biosensing Board from OpenBCI (https://shop.openbci.com/products/cyton-biosensing-board-8-channel?_pos=10&_fid=06e8267db&_ss=c) and the accompanying GUI. The Cyton Biosensing Board is a sensor used to obtain EMG data. To obtain hand signal data, the following setup (pictured below) was used:

IMAGE

As shown in the image, the 8 channel EMG sensing pins were connected to various locations along the user's forearm. Additionally, there was a ground connection connected to the user's elbow. This setup was chosen as I expected no muscular activation associated with a given hand signal at the elbow, so it provides a reliable reference point to compare each EMG channel to in order to isolate the muscular signal. In addition, the OpenBCI GUI allows for tuning the gain of the signal in order to 

## Invisible Map Overview
Invisible Map is an iOS app intended to help individuals who are visually impaired or blind navigate indoor spaces. To generate a map, users place AprilTags (similar to QR codes) throughout their space. Then, users walk through the space, scanning AprilTags and specifying notable locations in the space. While scanning, the app tracks the user's phone position to map out walkable areas and their position relative to the AprilTags. Then, the scanned data is sent to a backend server, where global positions of each feature of the map is calculated. The backend also optimizes the map as there are two main errors associated with the scanned data: odometry drift (errors associated with tracking the user's location over time) and tag detection inacuracy (errors calculating the tag's position and orientation relative to the user). Once processed, any user can load in the map and select any location to navigate to. During navigation, users scan AprilTags to localize themselves in the space. You can find a demo of the app's navigation [here](https://www.youtube.com/watch?v=EtkTXlaSYac).

In the past, there was no infrastructure to quantitatively assess map accuracy and, in particular, the optimized position of the AprilTags as there was no easily accessible ground truth data about their position, nor the support for integrating ground truth data with the backend system, which is what motivated my work.

## Obtaining Ground Truth Data
Previously, past members attempted several different methods to obtaining ground truth data, which varied from manually measuring AprilTag positions to using a floor plan to calculate their distances. However, none of this systems produced reliable data. As a result, two of my peers and I began researching other methods for obtaining ground truth data, which is when we found [rtabmap](http://introlab.github.io/rtabmap/). RTABMap is an open source mobile and desktop application that can be used to generate 3D point clouds of indoor spaces. An example point cloud is shown below:

![Sample RTABMap Point Cloud](https://i.imgur.com/6dOJEqJ.png)

Most importantly to us, the application has support for scanning AprilTags and recording their position in the 3D map. With a few modifications to the codebase, I was able to add functionality to exporting the position and orientation of each AprilTag in the global reference frame (relative to the position of the phone when the app is started) once a scan has finished. This allowed us to generate ground truth data on the position of AprilTags in our testing environment.

## Adding Backend Support for Ground Truth Data
Our next step was to add support for assessing how each generated map compared to ground truth data. This involved searching for a ground truth dataset every time a scanned dataset is being processed. If found, we added a step to the backend workflow that calculates a ground truth metric, which corresponds to how much the generated map's april tags vary positionally and orientationally from the ground truth data. We had to create a ground truth metric instead of simply computing the positional difference between corresponding tags because the two datasets were in two different reference frames. The ground truth metric is calculated by anchoring both datasets to corresponding tags and averaging the distance between each other tag. This is done using all the tags in the dataset as the anchor tag and averaging those values to yield the ground truth metric. 

We also created visualizations that show us how well the optimization adjusts the tags (whether they are moved towards or away from the ground truth tag position). An example of such visualization is shown below:

![Sample Ground Truth Visualization](https://imgur.com/tdXC6Bx.png)

For more information about our summer work, check out our [final presentation]()!

## Future Work
Now that we have a reliable system for evaluating the accuracy of maps generated by our backend system, we hope to conduct more systematic testing of the parameters being used in the backend. This will help us ensure that the parameters being used to generate maps allow for the highest map quality.

We would also like to create a "pseudo" ground truth metric that mimics the ground truth metric without the need for ground truth. Currently, we sweep through different confidence values on the positional and tag detection data and select the map with the lowest ground truth metric. However, this method is not practical as ground truth data is not always available, especially for the average Invisible Map user. For this reason, we are researching other methods for a similar evaluation. 

Finally, we would like to publish the work we have done, including information about the overall backend workflow and our future work on the pseudo ground truth metric.