# **Finding Lane Lines on the Road** 

## Writeup Template

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

My pipeline consisted of the following steps. To process an image, I first convert the image to grayscale.  I then apply a gaussian blur, follow by edge detection using the Canny algorithm.  The gaussian blur helps reduce noise when detecting the gradients in the Canny detection.  I mask an area of the image to identify the region of interest, and then apply a Hough Transform to that region to detect straight lines from the edges detected.   From these straight lines I am able to then draw two straight lines onto the image which represent the lane lines.  

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first splitting the lines into a group with positive slope and a group with negative slope.  I later refined this to look for slopes between 0.5 and 0.9 and -0.5 and -0.9.  I added all the line endpoints to 4 lists, a left_x, left_y, right_x, and right_y.  From these lists I then used np.polyfit to fit a straight line through the points and detect the slope and intercept of that line.  I also detect what the minimum y value is for each y list, and then draw a line from the bottom of the image to that minimum value at the slope calculated.  

[pipeline progression]: ./_render.jpg "Render"


### 2. Identify potential shortcomings with your current pipeline

My code currently doesn't handle curves in the road, and becomes apparent when running the pipeline through the challenge video.  I will need to come up with a different methodology to handle that scenario.  

I also have a lot of hardcoded values.  The test images were all in similar light environments and weather conditions, I imagine that those hardcoded values may struggle in different scenarios.  

### 3. Suggest possible improvements to your pipeline

I would need to come up with a new method to fit a line to a curved lane lane, whether I just do a higher order polynomial best fit or maybe just change the angle of the straight lines.  If I think further down the pipeline, specifically how the algorithm will take the lane line data to determine how to turn the wheels, then I might go with the higher order polynomial best fit.  The curve would then represent the trajectory of the vehicle with the wheels turned to some angle.  

To handle different lighting and weather scenarios, I would need to do more testing!  I could come up with example images in those scenarios and manually fine tune the hard coded values.  Depending on how much they vary, I could find a happy medium, or apply some kind of select case code structure, or have them vary dynamically and iteratively. 
