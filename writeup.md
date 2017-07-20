# **Finding Lane Lines on the Road** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consist of follwoing steps:
* set the url out of filename and directory parameters
* load the image
* create coordinates for mask
* apply mask on original image to check that mask ccordinates are set properly
* creatae grayscale image out of the original
* apply gaussian smoothing on graysclae image
* apply canny algorithm on smoothed grayscale image
* apply mask on canny image
* apply hough transformation on masked canny image
* creat a copy of the canny image
* apply hough as overlay on copied canny image (for check)
* apply hough as overlay on original image as final output

Beside these steps there is the possibility to output all images as results of these single steps for testing purposes



In order to draw a single line on the left and right lanes, I modified the draw_lines() function by following steps:
* as part of the given for loop through all relevant hough lines 
  * calculated the slope of the given line
  * applied a threshold of 0.2 to seperate the left lanes and right lines (not fitting the threshold were ignored)
  * append x and y coordinates in the segments arrays
  * append slope in the segments slope arrays
  * calculate mean for left and right coordinates segment and also for slope arrays
  * calculate the interceptions of the mean values
  * calculate new x ccordinates using min and max y -> defining to which part of the image the lines are extended
  * draw the lines



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be that the calculation of mean and resulting intersections is very fragil. For example, it happend to me that the first video was processed and the results were looking quite good. But processing the second video lead to an error stopping all further calculations. This was due to the fact that my parameter set was not adjusted reasonable. So the adjustments of parameters comes with a very high effort and try and error principle and does not guarantee optimal results for all given data.

Another shortcoming could be that the region of interest or mask is static. So every important mark / lane that is outside this region cannot be regarded and could lead to an uncertain situation for calculating the needed behaviour.

The image processing relies on the fact that all markers can be identified very good relying on bright colors, daylight and so on. Having this "static" environment of parameters, calculation and so we can assume that non optimal images would lead to a very defective lane deteion.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to set up a grid search for tuning optimal parameters without doing it by hand. Also it would be good to implement an exception handling so that defective states (detections) do not lead to a breakdown of the calculating system. 

Another potential improvement could be to have a non static region of interest that can react on the given situation. E.g. it is not said, that the lanes are fitting perfectly into the predefined mask, so it would be good to have the possibilities to react on such changes in the environment. This also applies not only to the region but also to changes in the given image data like darkend areas (e.g. shadows), heavy rainfall and so on. So an image preprocessing should also be applied as well.

For heavy load image data the calculation and math used could also be improved in terms of efficiency and process time.
