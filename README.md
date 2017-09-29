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

My pipeline consisted of 6 steps. First, I converted the images to grayscale, then I applied a guassian blur. With the noise reduced by the guassian blur, I applied the canny transform edge detection. Next a region of interest mask was used to remove edges we don't particulary care about. Now that we have the edges of interest, we run it through the hough lines process to connect the dots. Lastly, we draw the lines over the original image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by doing the following:

1. For each line, I first calculate slope by using formula (y2-y1)/(x2-x1)
1. I then determine if the line belongs on the left or right side and if it's greater than or less than zero
1. Now for each side, I do the following:
    1. Calculate the average x1, y2, x2, y2 using all the lines
    1. Then use the above to calculate the average slope using the same formula (y2-y1)/(x2-x1)
    1. Use the above to calculate the b intercept by using formula b = y-m*x
    1. With the b intercept, slope, and any y coordinate I can calculate x using the formula: x = (y-b)/m
1. The results were not bad but the lines would vary from frame to frame and would look jittery. I decided to 'smooth' the line by averaging the lines across frames. To do that, I did the following:
    1. Create a left and right queue and set a maximum number of frames to cache
    1. With nothing in the cache, I just return the line
    1. With something in the cache, I will average the current line with the lines in the cache and return the average

The following are the results of the pipeline over static images:

![alt text][image1]
![alt text][image3]

### 2. Shortcomings

* I found it difficult to determine the 'perfect' parameters for the canny and hough lines functions. Sometimes tweaking a parameter would produce better results with the static images but worse results with the video. I took a more conservative approach and would scale back the parameters to have more 'confident' lines knowing that they might be too short. The following are two examples:
  * ![alt text][image4]
  * ![alt text][image5]
* Another shortcoming are jittery lines across frames in the video. I had a hard time deciding how to fix this. I decided to use a queue as a cache for previous frames used for averaging. This turned out to work pretty well.
* The challenge video was indeed challenging -- I noticed the hood is visible and generating horizontal hough lines however, I couldn't remove them without affecting the other videos.

### 3. Improvements

* Perhaps I could run the same pipeline across different color channels and then average them. I'm wondering if other color channels would provide me with better results in tougher situations like shadows.
* Instead of extrapolating a straight line, perhaps we can fit to a polynomial line to take into account curve lane lines.
* I wish I could better determine the region of interest dynamically. For example, the region of interest for the challenge video is different but I've somewhat hardcoded for the other two videos.
