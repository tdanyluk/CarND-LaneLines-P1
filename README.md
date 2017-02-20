#**Finding Lane Lines on the Road** 

##Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:

* Make a pipeline that finds lane lines on the road

* Reflect on your work in a written report


[//]: # (Image References)

[image0]: ./pipeline/0.jpg "Original"
[image1]: ./pipeline/1.jpg "Grayscale"
[image2]: ./pipeline/2.jpg "Blurred"
[image3]: ./pipeline/3.jpg "Canny"
[image4]: ./pipeline/4.jpg "Region of interest"
[image5]: ./pipeline/5.jpg "Hough lines"
[image6]: ./pipeline/6.jpg "Combined"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. 

1. I converted the image to grayscale.

2. I applied gaussian blur.

3. I applied Canny edge detection.

4. I applied a quadrilateral region of interest filter.

5. I applied Hough line search algorithm.

6. I combined the original image with the image of the lines drawn by the draw_lines() function.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:

* Drop all segments which are exactly vertical (this is unlikely).

* Drop all segment which are too horizontal (absolute slope < 0.25).

* Determine the side (left/right) of each segment by its position (left/right part of the image) and slope (positive/negative).
(Some segments are neither left nor right, they are dropped.)

* Calculate the average of the segments on each side, by averaging their upper and lower ends. 
(This gives us a segment which is near the lines of the original segments, and has similar slope to them.)

* I extrapolated the segment to go from the top of the region-of-interest to the bottom of it.

The pipeline in images:

![Pipeline 0][image0]
![Pipeline 1][image1]
![Pipeline 2][image2]
![Pipeline 3][image3]
![Pipeline 4][image4]
![Pipeline 5][image5]
![Pipeline 6][image6]


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the shadow of a tree or other object would be so contrasty that it would show up in the Canny image. 
It might yield segments which would modify the position of the final lane lines. 

Another shortcoming could be that I don't support scenarios when the car turns to the side, so that the lane lines are almost horizontal.

Also the algorithm cannot correctly handle it when a car is switching lanes.


###3. Suggest possible improvements to your pipeline

A possible improvement would be to consider color (hue) in addition to intensity when calculating the Canny image.

Another potential improvement could be to make draw_lines more intelligent, 
eg: it should sort the lines into almost colinear groups and only consider the 2 groups with the most/longest lines.
So smaller signs or shadows would not interphere as in the average calculation.
