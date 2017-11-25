**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[grayscale]: ./examples/grayscale.png "Grayscale"
[gaussian]: ./examples/gaussian.png "Gaussian"
[canny]: ./examples/canny.png "Canny"
[region]: ./examples/region.png "Region of Interest"
[hough]: ./examples/hough.png "Hough"
[output]: ./examples/output.png "Extrapolated Lines"


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 
* I converted the images to grayscale
![Grayscale][grayscale]
* Applied Gaussian with kernel size 5 to smooth the image
![Gaussian Smoothing][gaussian]
* Applied Canny Edge detection to detect the edges in this image. I played around with thresholds and found 200 to 255 as optimal values to find the lines.
![Canny][canny]
* I selected a region of interest in the bottom half of the image by forming a triangle in the lower half of the image
![Region of Interest][region]
* Applied Hough transform in this region to identify the line segments. I played around with max and min line gap to identify all the points on both sides of the traffic lane.
![Hough Transform][hough]
* Modified draw_lines() to identify the left segment and right segment.
I could identify that the left segment has a positive slope while the right segment has negative. I ignored all the line segments which have a slope of zero. I calculated slopes on segments from the Hough transform and averaged it. Took a mean of all points on each segment. 
I, now, have to draw a line passing through this "mean point" having mean slope that begins from bottom of the image. This is true for the right segment as well only that the slope and mean point would change.
This can be seen in the following image.
![Extrapolated lines][output]


### 2. Identify potential shortcomings with your current pipeline


* When trying on challenge video, I realised that my pipeline fails when the road is pretty lighter in shade and the lane color is pretty much the same as the road color.

* Another one is that in a given video, the line transition is not smooth. The lines are wobbly in each frame since it recacluates the extrapolation for every frame in the video.


### 3. Suggest possible improvements to your pipeline

* To rectify the second mistake, I think I need to look at the previous line location while extrapolation so that the transition is smooth.

* Similar technique can be used to fix the first issue mentioned above. When the next frame data is very different from previous frame, ignore it.
