---
name: EMG Sensing Circuit Board
tools: [KiCAD, Arduino, Python]
image: https://i.imgur.com/3RoDsEX.jpg
description: During my sophomore year, I developed a PCB designed to measure 4 separate Electromyography signals.
---

# EMG Sensing 
Electromygraphy is a medical tool used to evaluate the health of individual muscles and the nerve cells that dictate their movement. The intention of this project was to design a four-channel surface electromygraphy (sEMG) printed circuit board in order to process real-time electromyographic signals generated from a user’s muscle movements. The board was intended to communicate with a user’s computer via Bluetooth and Serial to provide flexibility to the physical setup, along with portability for ease of access to EMG information.

## Motivation
There were two main motivations for this project:
1. During my first year, I had the opportunity to work with an already built EMG signalling board, but wasn't fully knowledgeable about how it worked. This project allowed me the opportunity to study the system at a lower level.
2. This was my final project for Eclectronics (an Olin coursed intended to teach all aspects of printed-circuit board design and prototyping). As such, I wanted to a choose a project that advanced what I had already learned through the course.

## Implementation
### Hardware
To start, I had to design a signal processing module to analyze the raw EMG signal from the EMG electrodes. After consulting with our professor and my group, we settled on using a differential amplifier, followed by a high pass filter and amplifier, rectifier, level shifter, and finally a low pass filter and amplifier. We started with a differential amplifier as we wanted to analyze the difference between two EMG electrodes for better signal clarity. Raw EMG signals are typically 10 mV and range between 20 Hz and 500 Hz. Following the differential analysis, we wanted to filter out noise, which led to the low pass and high pass filter. We also included a rectifier and level shifter since we used a 1.65V reference for our signal in the differential amplifier and wanted to expand the signal to span the full 0-3.3V range that our microcontroller (RP2040) could read to maximize measuring precision. To esnure that the design would work as intended, we simulated the processing module in LTspice and tested it under a wide variety of input signals.

Once the schematic was complete, the next step was to draft the layout. The first iteration was a two layer board, where the top layer was for power and signals, while the bottom layer was for ground and signals. However, when routing traces, we found that there were several digital signals in close proximity to each other. This would lead to significant cross-talk between the lines, causing logical errors when using the board. To solve this issue, we chose to move to a four layer board design to offer more space and separation for routing sensitive signals. The board stack up, from top to bottom, was as follows: power and signal, ground and signal, power, ground and signal. This led to a far more compact, and isolated design that we were confident in. The full layout can be seen below.

![Final Layout for EMG Sensing Board](https://i.imgur.com/w46hzag.png)

### Software
Once the hardware was designed and sent for fabrication, we started designing the software. The software had two main components: the firmware that would run on the RP2040 to continuously process and send readings from the 4 ADCs connected to the 4 EMG channels, and the laptop interface that would receive and visualize data for each of the 4 channels. On the firmware side, we chose to use a hardware abstraction library, called the arduino-pico project, which allowed us to develop the image in the Arduino IDE. Within the image, we initialized each of the 4 ADC pins and continuously read and print them to the serial connection, with a marker that identifies which channel is belongs to.

On the software side, we initialize a dictionary to store the EMG data, along with the time it was received, for every EMG channel. The data is received via serial and processed using the pyserial library. Then, using matplotlib, we create a dynamically updating plot that displays the most recent 10 seconds of data for each channel. A sample visualization is shown below.

![Sample Python Visualization of EMG](https://i.imgur.com/doJ9o3h.png)

## Results
With the current hardware, we have received inconsistent results in processing and analyzing EMG data from the sensing board. At times, we are able to properly receive data. This is demonstrated in our [one-channel demo](https://www.youtube.com/watch?v=9YESAbq6-Ng). However, there are issues with running all four channels working synchronously. During some tests, all four channels can be properly monitored for a brief period of time, but, in most tests, only one or two channels yield expected results while the others constantly remain undetectable. The full demo of the current state of the system can be found [here](https://www.youtube.com/watch?v=dAzlrd0NfQ0&feature=youtu.be).

Based on these results, we pinpointed a few limitations that could be causing the inconsistent behavior that we are seeing. Those include:
1. Misplacement of the EMG electrodes on the muscle. If the electrodes are placed improperly, they may pick up the same signal, causing their difference to be always be 0, leading to no signal.
2. Inadequate contact between the EMG electrode and the muscle. If proper contact isn’t established with the muscle, then the signal will be weaker, or not detectable at all, leading to improper results.
3. Delay in serial communication. We found that, at times, there is a delay between a physical movement and the visualization of that response. We believe this is either because of how the serial communication is implemented or a physical limit to how fast serial communication can be established.

We hope to continue developing this system to make it more reliable and produce more consistent results.

## Reflection
This project was an excellent learning experience for me. It was my first hardware project that wasn’t firmly scaffolded by external design constraints, allowing me to explore new avenues in electrical engineering. This included learning a lot about setting my own design constraints based on a specific function I want to implement. The main challenge I faced during this project was learning how to debug an embedded system. Numerous times throughout the process, I wasn’t sure if there was a hardware or software issue causing unintended results, which forced me to learn new debugging tools to
isolate the problem to one or the other and act accordingly.

Additionally, this project taught me the value of implementing features to de-risk certain aspects of the project in order to maximize the chance of meeting an intended deliverable. For example, we wanted to establish a bluetooth connection between the sensing board and a computer, but due to time constraints, were not able to implement this. Thankfully, we had de-risked the bluetooth communication by also including a serial interface to receive data, allowing us to still extract and visualize the processed EMG signal.

For more details, checkout out the [hardware](https://github.com/ayushchakra/emg-sensing/tree/main/hardware), [software](https://github.com/ayushchakra/emg-sensing/tree/main/software), and [full report](https://github.com/ayushchakra/emg-sensing/blob/main/EMG_sensing_final.pdf) for this project!