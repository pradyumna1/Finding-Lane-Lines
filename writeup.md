# **Finding Lane Lines on the Road** 

### This was a great opportunity to refresh computer vision fundamentals. Here is my approach on finding lane lines: 

---
This project had following goals
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[test_images_output]: ./test_images_output/final_test_images_output.png "All images output"

[pipeline_stages]: ./test_images_output/solidYellowCurvePipeline.png "Pipeline Stages"

---

### Reflection

### 1. Pipeline Description

My pipeline consisted of 5 steps:
- Convert the images to grayscale to directly work with pixel intensities (instead of 3 different colors)
- In order to reduce abrupt changes in consecutive pixels, smoothen the image by using gaussian noise filter
- Apply Canny edge detection on this smooth grayscale image.
- The image obtained from above step will clearly lane lines standing out. However it will also contain other
  edges, so we will mask the image to obtain only the region containing lane lines.
- Then we will use the Hough transformation to obtain the coordinates of line segments in the masked image.
  This coordinates will be used to approximate the equation of lane lines. The approximation was done in draw_lines() function
  function.
  
![alt text][pipeline_stages]

Earler draw_lines() funtion would take the coordinates of line segment and draw them on the image.
In order to draw a single line on the left and right lanes, the new draw_lines() followed this steps:
- calculate slopes and intercepts from coordinates.
- filter slopes with absolute values between 0.5 and 1.
- separated left and right lines using the sign of their slope.
- calculated mean slope and mean intercept for both left and right line.
- plot the lines with mean slope and intercept, with bounding lines same as the masking polygon's upper and bottom edges.

![alt text][test_images_output]


### 2. Identify potential shortcomings with your current pipeline

I anticipate following shortcomings with my pipeline:
- One potential shortcoming would when the lanes are curving too fast, i.e., when the car
  is on a sharp turn.
- When there are other objects on the road, for eg. a road patch, or a shadow
- When the color of lanes is fading, it would be hard to detect edges 

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to extrapolate various segments instead of points.
This would take care of sharp turns. I think it could be done by separating various very close slopes after
hough transformation, plotting lines for them, and then connecting their end points.
 
