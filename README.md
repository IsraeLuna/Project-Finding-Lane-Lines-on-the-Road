# Project-Finding-Lane-Lines-on-the-Road
First project of Term 1 for Udacity's Self Driving Car Nanodegree

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline was bluit as described by the steps below:
 
 First, the image we wished to process was converted to grayscale

![alt text][image1]

After that a Gaussian Blur was applied to the grayscale image (such image format is the required input), it uses a normal distribution to change pixel colors in order tu fit an average over many "regions".


With the now blurry image, the Canny edge detection function was used to create a black image, with white edges detected by a great change in pixel.

Because we know the lane lines will always be found in a specific shape and area, we define that as our region of interest in order to ignore undesired edges making noise in our image.

Then, with the Hough Transform solid lines were detected by finding which of the edges shared similar equations (y = mx + b), by switching from coordinates in the image array to polar coordinates.

By know, the processed imaged could show the lines along the lane very accurately. But we would like both lines to be cointinuous along the path and not just the solid line on the side of the road. So, the following cycle was implemented:

Since we already have an array which gives us the starting and ending points of the lines, which is the output of the Hough Transform, we use those points to find the lines' equations and average the slope (m) and constant (b) to finally paint every pixel in the range of the equation within the boundaries previously established.

As a result we get two straight lines which fit precisely where they should.

Finally the pipeline is setup to call for the image processing for a test video and the test images.


### 2. Identify potential shortcomings with your current pipeline

Though the pipeline performed very well on the first to videos it came short when it processed the challenge video. It lost the lane lines at several times along the road. This was in part due to the point of interest not changing even as the image shape did, but also because at some frames the hough transform function did not find lines and therefor no pixels were to be painted. A quick solution for that could be to simply put a condition to "recover" previous equations for the line when no new segments can be painted, with the risk that a very drastic change of path would have misleading lines.


### 3. Suggest possible improvements to your pipeline

A lot of improvements can be made, or other solutions could be implemented. The pixel coloring is very inefficient and unnecessary, since, as said previously, one only need the first and last point of a line in order to connect them. So that would be a way to reduce computing time, not having to paint a single pixel for each iteration of the cycle. 

Another detail is to define more "functions" for each process: Gaussian Blur, Canny Edge Detection, Hough Transform, and so on. That would make the code much clearer to anyone trying to interpret it.

Other solution for the discontinuous lines would be to use linear regression, by using the points from the Hough Transform method to estimate a line which minimizes the error of deviations from the points against said equation.
