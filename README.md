# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve.jpg "solidWhiteCurve"
[image2]: ./test_images_output/solidWhiteRight.jpg "solidWhiteRight"
[image3]: ./test_images_output/solidYellowCurve.jpg "solidYellowCurve"
[image4]: ./test_images_output/solidYellowCurve2.jpg "solidYellowCurve2"
[image5]: ./test_images_output/solidYellowLeft.jpg "solidYellowLeft"
[image6]: ./test_images_output/whiteCarLaneSwitch.jpg "whiteCarLaneSwitch"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

1. Grayscale
2. Guassian blue
3. Canny
4. Region of interest
5. Hough lines
6. Merge image

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

1. Calculate slope
2. Determine left or right side
3. For each side, I do the following:
    a. Calculate avg x1, y2, x2, y2
	  b. Use the above to calculate the avg slope
	  c. Use the above to calculate the b intercept b = y - m*x
	  d. With the b intercept, slope, and any y coordinate I can calculate x using the formula: x = (y-b)/m
4. The results were not bad but the lines would vary from frame to frame and would look jittery. I decided to 'smooth' the line by average the lines across frames. To do that, I did the following:
	  a. Create a left and right queue and set a maximum number of frames to cache
	  b. With nothing in the cache, i just return the line
	  c. With something in the cache, i will average the current line with the lines in the cache

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]


### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when ... 
Another shortcoming could be ...

1. Tweaking parameters for canny and hough lines.
2. Horizontal hough lines. Had to tweak the region of interest.
3. Jittery lines across frames in the video. Had to figure out how to cache previous frames used for averaging.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...
Another potential improvement could be to ...

1. Perhaps using the same logic across different color channels and then averaging them.
2. Instead of extrapolating a straight line, perhaps we can fit to a polynomial line to take into account curve lane lines.
