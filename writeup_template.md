# **Finding Lane Lines on the Road** 
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Lane Detection Pipeline

The pipeline has the following steps:
* **Color Masking:** This is an optional step. First, the results are obtained without applying the color mask. The results are 
for the exercise videos. But the pipeline without color masking performs poorly on challenge video. In second part the results are
obtained with color mask.
The color mask detects yellow and white color in HSL color space. The HSL color space is used as white and yellow color lanes are very
distinguishable in HSL color space in bright sunlight and shadows.

* **Gray Scaling:** The image obtained after above optional step the color image is converted to gray scale.

* **Gaussian Blur:** The image is smoothed using a Gaussian smoothing filter, this step will smooths out any rough edges in the image and it will not appear during Canny edge detection

* **Edge Detection:** Canny edge detection is applied to detect edges in the blurred image. A helper function is also created that adaptively determine low and high thresholds of gradient which will be used in Canny edge detection.

* **Region Masking:** The above step will detect edges in complete image, to focus on the region where the lane markings a region mask is created and applied on the image.

* **Finding Line Segments:** From edges form line segments using *HoughLinesP* method of opencv. Several parameters are need to be set for the fucntion to work well. Draw line segments using *draw_lines()* function.

* **Combining:** The line segments image is combined with original image to produce final result.

Overview to *draw_lines()* function to extrapolate line over lane using detected line segments:

* **Seperating line segments:** The line segments are seperated as belonging to left lane and right lane based on their slope values. Positively sloped line segments belong to right lane and negatively sloped line segments belong to left lane.

* **Calculating parameters and weights:** For each line segment, its parameter values (slope,intercept) and its weight value are calculated.
Weight of a line segment is an increasing function of its eucledian length. The results are stored in a list.

* **Extrapolated line parameters:** The parameters (slope,intercept) for the extrapolated line for left and right lane are calculated as weighted average of the line segments parameters with the weights  and corresponding parameters calculated in previous step.

* **Adding to the frame list:** The parameters for extrapolated line obtained for left and right lane respectively are added to the previous frame list that stores the similar parameters from previous video frames.

* **Averaging over frame list:** To draw left and right lane lines, the parameters for the corresponding lane is obtained by averaging over the frame list modified above. This step and the previous step help in drawing line over frame that looks smooth frame by frame, by smoothing any minor jerks in the line parameters.

* **Drawing line:** Finally the lines are drawn using the avereged parameters obtained above. The y-coordinate values are chosen and corresponding x-coordinate values are obtained using the parameters determined above.

### 2. Shortcomings

I believe this is very basic lane detection mechanism and needs improvement:

* **Color Mask:** In the second part of the work to do well on the challenge video I have used a color mask for detecting yellow and white lane markings. It was a struggle to find the approriate color range. The pipeline is sensitive to color range. In different lighting condition it may not work very well.
* **Fitting Straight Lines:** Strainght line are fitted for the lanes, this pipeline will fail to detect the curvature of lane markings that will result in poor result.

* **Traffic:** If there is lot of traffic on the road it may affect the result, especially if the vehicles ahead are covering the lane markings the result will not be very good.


### 3.Improvements
Improvements:
 
 * **Non linear polynomials:** Instead of fitting straight lines, several non linear polynomials can be tried and based on the value of residuals appropriate degree polynomial can be chosen to represent the lane markings.
 * **Adaptive parameters selection:** For hough transform the parameters were determined by hit and trial method, adaptive strategy can be designed to determine the parameters to be used. 
