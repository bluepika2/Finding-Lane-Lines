# **Finding Lane Lines on the Road** 

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project I will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

To complete the project, several methods will be applied to images to detect left and right lane on the road. The methods will be following:

- Grayscale image conversion
- Gaussian Smoothing
- Canny edge detection
- Image masking
- Hough transform

All codes is included in `P1.ipynb`.


The Project
---

[//]: # (Image References)

[image1]: ./test_images_output/solidYellowLeft.jpg "solidYellowLeft"
[image2]: ./test_images_output/solidYellowLeft_rev.jpg "solidYellowLeft"
[image3]: ./test_images_output/pipeline.png "pipeline"
[video1]: ./test_videos_output/solidYellowLeft.mp4 "video"

---

### 1. Pipeline

My pipeline consisted of 5 steps. 
1. converted the images to grayscale and applied Gaussian smoothing with kernel size 5
2. applied Canny edge detection with appropriate thresholds
3. created vertices to mask image only for interested region using four sided polygon

![alt text][image3]

4. applied Hough transform on edge detected image. In this step, I did not modify the `draw_lines()` function, so my image was as below.

![alt text][image1]

5. In order to draw a single line on the left and right lanes, I modified the draw_lines() function.
I differentiated left and right line based on slope value from ((y2-y1)/(x2-x1)), and average this value for each left and right lane.
I also needed to make every averaged value to integer to plot these two lines. Afterwards, I did extrapolation based on my pre-defined polygon from bottom to top. Trick part here is our x and y coordinates are different from normal x-y plane, which means our y value goes from top to bottom. Therefore, I needed to extrapolate such as x = my + b, not y = mx + b.

Below is another image after applied modified `draw_lines()` function.

![alt text][image2]


### 2. Identify potential shortcomings with your current pipeline

My modified `draw_lines()` function worked well for all images. However, when I applied the function to video, my detected lanes sometimes were fluctuating especially for "solidYellowLeft" video. In order to make lane detection more reliable, it needs more robust function.
Also, if camera position is changed, this pipeline using pre-defined region of interest should be also changed.
Furthermore, I think this pipeline only works for straight line. If our vehicle is driving on some curvy road, lane detection might need another strategy.


### 3. Suggest possible improvements to your pipeline

If I can make my draw_lines() function more robust for any different images, then lane detection in video will be more consistent.
My pipeline was applied not only to images, but also to video as following:

Here's a [link to my video result](./test_videos_output/solidYellowLeft.mp4)



