---
name: Hole in the Camera
tools: [Python, Neural Network]
image: https://i.imgur.com/fsxZ60Y.png
description: Hole in the Camera is a fun, at-home game that challenges users to conform their body into seven distinct "camera holes".
---

# Hole in the Camera
Hole in the Camera was a game I developed as part of my first-year Software Design Final project. The game is modelled after the popularo Nickelodean game show, [Hole in the Wall](https://en.wikipedia.org/wiki/Hole_in_the_Wall_(American_game_show)). Hole in the Camera is an at-home friendly version of the popular game show. Each competitor is presented with 7 holes (camera masks) that they need to fit into. Their score for each trial is based on how well their body position fits into the mask (out of 100). The full project can be found [here](https://olincollege.github.io/hole-in-the-camera/). 

## Technical Overview
### OpenCV
OpenCV is an open source computer vision library that allows software developers to extract and manipulate a camera stream. In our game, we use OpenCV to access the user's camera and continuously stream frames into our backend to be analyzed. Additionally, OpenCV was implemented in order to create the masks for each trial that the user has to fit into. An example of a mask is shown below:

![Sample Mask](https://i.imgur.com/5LCzJjv.png)

This mask was created by filtering the camera output based on specific HSV values, and then eroding and dilating them sufficiently until they are rendered to being black and while iamges representing where a human was in the camera.

### Pygame
Pygame allows us to display our game user interface to players. We employ pygame as our view, displaying instructions and results to users as they play the game. Pygame also allows us to constantly display the user’s position in the camera frame through the OpenCV output for them to adjust themselves as they try to fit into the displayed hole. In addition, pygame enables us to play music and sound effects during game play to enhance the user experience.

### OpenPose
OpenPose is a public implementation of the Deep pose algorithm that can be used to determine a human's joint positions in a given image. An example OpenPose analysis is shown below:

![OpenPose Example](https://i.imgur.com/6SRSHZY.png)

This diagram shows the calculated positions of a user's joints from OpenPose, which also included finger joint analysis.

Our game's entire backend system for evaluating how close a user matched the mask they needed to fit into. Once the timer has concluded, the final frame is processed. Once the joint positions are calculated, they are compared against the expected joint positions (which is stored with the mask). This error is scaled to determine the user's final score.

## Project Experience
Through this project, I was able to make several strides as a programmer, especially with the support of my two team members. These are my primary takeaways from this experience:
1. The value of unit tests and proper documentation. Perhaps the most important takeaway, I learned that unit tests and properly documenting code. Unit tests allowed us to ensure that our code was functioning as intended, especially when faced with edge cases. Documentation allowed for us, as a team, to properly communicate our progress and modularize our code to promote clarity.
2. The power of a neural network. Even though we barely scratched the surface of its use cases, we were amazed by the accuracy of OpenPose, which only further motivates me to pursue more use-cases of machine learning.
3. OpenCV. OpenCV was also such an instrumental tool to this project's success. This project made me far more comfortable with reading OpenCV's documentaiton and being able to implement their algorithms for my own use-cases.

If you want to play our game, check out the "Playing the Game" section of the [project webpage](https://olincollege.github.io/hole-in-the-camera/)!