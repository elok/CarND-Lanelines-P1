# **Finding Lane Lines on the Road** 

---

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

### 1. Pipeline Overview

My pipeline consisted of 6 steps. First, I converted the images to grayscale, then I applied a guassian blur. With the noise reduced by the guassian blur, I applied the canny transform edge detection. Next a region of interest mask was used to remove edges we don't particulary care about. Now that we have the edges we care about, we run it through the hough lines process to connect the dots. Lastly, we draw the lines over the original image.

The following are the results of the pipeline over static images

![alt text][image1]
![alt text][image3]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by doing the following:

1. For each line, I first calculate slope by using formula (y2-y1)/(x2-x1)
2. I then determine if the line belongs on the left or right side if it's greater than or less than zero
3. Now for each side, I do the following:
    a. Calculate the average x1, y2, x2, y2 using all the lines
	  b. Then use the above to calculate the average slope using the same formula (y2-y1)/(x2-x1)
	  c. Use the above to calculate the b intercept by using formula b = y-m*x
	  d. With the b intercept, slope, and any y coordinate I can calculate x using the formula: x = (y-b)/m
4. The results were not bad but the lines would vary from frame to frame and would look jittery. I decided to 'smooth' the line by averaging the lines across frames. To do that, I did the following:
	  a. Create a left and right queue and set a maximum number of frames to cache
	  b. With nothing in the cache, I just return the line
	  c. With something in the cache, I will average the current line with the lines in the cache and return the average


### 2. Shortcomings

One potential shortcoming would be what would happen when ... 
Another shortcoming could be ...

1. Tweaking parameters for canny and hough lines.
2. Horizontal hough lines. Had to tweak the region of interest.
3. Jittery lines across frames in the video. Had to figure out how to cache previous frames used for averaging.

![alt text][image4]
![alt text][image5]


### 3. Improvements

A possible improvement would be to ...
Another potential improvement could be to ...

1. Perhaps using the same logic across different color channels and then averaging them.
2. Instead of extrapolating a straight line, perhaps we can fit to a polynomial line to take into account curve lane lines.
