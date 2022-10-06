<!-- ---
name: Hand Signal Detection
tools: [Unity, Data Networking, Neuropype]
image: 
description: 
--- -->
<!-- 
# Hand Signal Detection
## Project Overview
During my first year at Olin, I joined the Human Augmentation Laboratory, which is investigating how Augmented Reality can be integrated into daily life to optimize daily life. Towards this goal, I was tasked with developing a proof-of-concept system that can analyze a user's hand signals and trigger a corresponding response on our MagicLeap (INSERT LINK HERE), an Augmented Reality Headset. The project is split into two main phases: backend training and real-time classification.

## Backend Training
In order to classify hand signals in real time, we needed to develop a large dataset and train a backend machine learning model. This was done by extracting electromyography data from the hands of three different individuals using a Cyton Biosensing Board from OpenBCI (https://shop.openbci.com/products/cyton-biosensing-board-8-channel?_pos=10&_fid=06e8267db&_ss=c) and the accompanying GUI. The Cyton Biosensing Board is a sensor used to obtain EMG data. To obtain hand signal data, the following setup (pictured below) was used:

IMAGE

As shown in the image, the 8 channel EMG sensing pins were connected to various locations along the user's forearm. Additionally, there was a ground connection connected to the user's elbow. This setup was chosen as I expected no muscular activation associated with a given hand signal at the elbow, so it provides a reliable reference point to compare each EMG channel to in order to isolate the muscular signal. In addition, the OpenBCI GUI allows for tuning the gain of the signal in order to  -->