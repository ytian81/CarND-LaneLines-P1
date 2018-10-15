# **Finding Lane Lines on the Road** 

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My lane annotation pipeline is fully represented by function `lane_annotator`. It has two parts: the first part is the parameter and the second part is implementation. The pipeline has 5 steps: 

* Convert color image into grayscale
* Use Gaussian filter to reduce noises and spurious gradients
* Use canny edge detection algorithm to find all edges
* Mask out uninterested region.
* Use Hough transform to find consistent lane lines

Masking out uninterested region is intented to be executed before hough transform because edges in the uninterested may reasult in unexpected noisy branches on main lane lines.

In order to draw a single line on the left and right lane, I use line slope to firstly differentiate left lines from right lines and then use least square linear regression to fit a single line for each lane. Challenge video needs further filtering out wrong orientation lines. By heuristic lane lines are almost vertical from front camera, so any lines having absolute slope less than 0.5 are filtered out. 


### 2. Identify potential shortcomings with your current pipeline

The current pipeline works well for provided videos. However, I could imagine it works bad for curved lane lines. Supposing car is making a sharp turn, most part of visible lanes from front camera would be highly curved. Vanilla hough transform can only capture straight lines. High degree hough transform and linear regression might be used instead. 

Another shortcoming is that annotated lanes are somehow flickery. By heuristic we could hold last tick's lane annotation if we don't have new annotation for current tick. 

The region of interest mask is completely based on intuition. If camear angles are modified, this pipepline might not be able to work properly. Lighting condition also has severe impact on detection reliability. 


### 3. Suggest possible improvements to your pipeline

* High degree hough transform and linear regression
* Bookkeep last tick's lane annotation if current lane perception is null
* Calibrate the camera and use lidar to detect lanes