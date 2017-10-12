# **Finding Lane Lines on the Road** 


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road

---

### Reflection

### 1. Pipeline for finding lane lines on the road

My pipeline consists of following steps
 
a) Apply Color Thresholds

Given input image is a color image. It has pixels whose value vary from 0 to 255 in R, G and B channels.
We are only interested in identifying lanes. Color thresholds are applied in such a way that only 
lane related information in the image is retained and rest of pixels are blacked out.
This step removes unnecessary information in the image.

[image1]: ./examples/start.jpg " Color Image"
[image1]: ./examples/color_threshold.jpg "After applying color thresholds"

b) Canny edge detection 

Next in the pipeline, we prepare the image for canny edge detection. We use canny edge detection
to better identify lane lines in the image. The strength of an edge is defined by how different
the values are in adjacent pixels of an image. Strength of an edge is the strength of the gradient
Computing the gradient gives us thick edges. We will thin out edges using canny edge algorithm
This gives us individual pixels that follow strong gradient.

The following steps are performed to better prepare the image
for Canny edge detection

    1) Convert the image to grayscale.
       Canny edge detection works on pixel intensities. Converting an image to grayscale 
       retains only intensity information. When an image is converted to grayscale, 
       pixels are in different intensities of gray. this works better for canny edge detection.
       
    2) Apply Gaussian Smoothing
       Gaussian Smoothing suppresses spurious gradients. OpenCV Canny function applies 
       Gaussian smoothing/blur internally but it is not changeable parameter.
       
   That completes the preparation steps for Canny Edge detection algorithm
   
   Now, we apply Canny 
   
   [image1]: ./examples/canny.png "Image after applying Canny Edge Detection Algorithm"
   
c) Region Masking

Assuming the front facing camera that took the image mounted in a fixed position 
on the car, lane lines will always appear in same general region of the image.
Im applying a trapezoid mask to consider pixels in the region where we expect to
find the lane lines and mask everything else in the image.

[image1]: ./examples/roi.png "Image after applying Region Masking"

d) Apply Hough Transform to identify line segments

After detecting edges from Canny edge detection, we need to find out 
lines in the image. We use Hough transform to identify intersecting lines
in Hough space which translates to finding lines in the image space.

Hough transform gives the co-ordinates of lines which make up left and right lanes. 
In order to draw single line on the left and right lanes, I followed following procedure.


1) Classify the lines into left and right lanes based on positive and negative slope. 
   I have also considered threshold for slope to eliminate outliers.

2) Determine the equation of line for left lane and right lane using opencv polyfit method.
   This gives us average/mean slope of both lanes.

3) Extrapolate the line segments to  bottom of the image using the line equation

4) Identify the top most line segment of either lanes and extrapolate both lanes to top most point


[image1]: ./examples/endofpipeline.png "End result of pipeline"


### 2. Potential shortcomings with your current pipeline

Steep curves
Current pipeline does not work effectively if there are sharp turns
or steep curves and lines go slightly off from the actual lane markings

Shaded regions
Current pipeline might not work as expected if there are shaded regions
When there are shaded regions lanes might not be identified with high accuracy




### 3. Suggest possible improvements to your pipeline

Currently we are only considering RGB color space.
One possible improvement would be consider other color spaces
such as LAB and HSV for different lighting scenarios.
This helps with detection of certain colors such as yellow
better than RGB color space.


Another potential improvement could be to consider average line slope 
from previous frame and compare it with current frame to smoothen lane detection on
steep curves.
