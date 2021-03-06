# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[fig1]: ./test_images_output/line_segments/solidWhiteCurve.jpg
[fig2]: ./test_images_output/line_segments/solidWhiteRight.jpg
[fig3]: ./test_images_output/line_segments/solidYellowCurve.jpg
[fig4]: ./test_images_output/line_segments/solidYellowCurve2.jpg
[fig5]: ./test_images_output/line_segments/solidYellowLeft.jpg
[fig6]: ./test_images_output/line_segments/whiteCarLaneSwitch.jpg

[fig7]: ./test_images_output/lane_marks/solidWhiteCurve.jpg
[fig8]: ./test_images_output/lane_marks/solidWhiteRight.jpg
[fig9]: ./test_images_output/lane_marks/solidYellowCurve.jpg
[fig10]: ./test_images_output/lane_marks/solidYellowCurve2.jpg
[fig11]: ./test_images_output/lane_marks/solidYellowLeft.jpg
[fig12]: ./test_images_output/lane_marks/whiteCarLaneSwitch.jpg

[vid1]: ./test_videos_output/solidWhiteRight.mp4

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

#### My pipeline is as follows. 

* I converted the images to grayscale.
* I applied Gaussian smoothing with kernel size of 5 to suppress noise.
* I applied Canny edge detection on the smoothed image with low threshold being 80 and high threshold 180.
* I specified the region of interest as a trapezoid and used it to filter out all the edges outside of the region. Since the camera is not exactly centered, my mask was shifted as well.
* I ran Hough transform with rho = 2, theta = pi / 180, threshold = 20, min_line_length = 10 and max_line_gap = 10.
* I superimposed the result with the original image using weighted_img() helper function.

![alt text][fig1]
![alt text][fig2]
![alt text][fig3]
![alt text][fig4]
![alt text][fig5]
![alt text][fig6]

#### In order to draw a single line on the left and right lanes, I modified the draw_lines() as my_draw_lines().

* I separated the hough line segments into two groups according to their slope and position (left/right) on the image.
* I applied linear regression on the lines' endpoints, estimating the two line equations, respectively.
* According to the estimated line parameters, I drew only one line on the left and right lanes at their reasonable position.

![alt text][fig7]
![alt text][fig8]
![alt text][fig9]
![alt text][fig10]
![alt text][fig11]
![alt text][fig12]

#### Test on Videos

The pipeline above works well on solidWhiteRight.mp4 and solidYellowLeft.mp4. The resulting videos are stored in the folder test_videos_output.

#### Optional challenge

In order to take on the challenge video clip, I did the following modification to my pipeline.

* I skipped the grayscale step for better contrast.
* I made slope filtering more strict, i.e. to eliminate the horizontal edges.
* Instead of using a simple linear regression, I incorporated RANSAC algorithm for outlier rejection. 
* I still fit endpoints using linear regression but only on the inliers. 
* RANSAC regression was implemented with scikit-learn library.
* If no lane detected, skip this frame to prevent raising errors. (Not get used in the challenge actually.)  
* The resulting video are stored in the folder test_videos_output, named challenge.mp4.
* The enhanced pipeline still works well on both solidWhiteRight.mp4 and solidYellowLeft.mp4.


### 2. Identify potential shortcomings with your current pipeline

* RANSAC is an iterative algorithm, and therefore is potentially inefficient. 

* The real-world pavement could be even more challenging than what happens in the video challenge.mp4, such as a ruleless break in cement concrete pavement. The current pipeline is using slope filtering which could fail on vertical breaks.  


### 3. Suggest possible improvements to your pipeline

* A possible improvement for the first shortcoming would be to enhance the preprocessing to make the input line segments to RANSAC as few as possible. So, RANSAC algorithm would work very fast on a small number of sample points. 

* A potential improvement for the second shortcoming could be to use color segmentation such as modeling the color probability.





