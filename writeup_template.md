#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of the following steps. First, the image being processed is converted to grayscale. Next, a Gaussian blur is applied to the grayscale image in order to reduce noise. Then the Canny function is applied to the resulting image in order to get the image edges. To reduce the number of edges a mask is applied to the image which keeps only the region containing the lanes. This is achieved by defining a polygon around the lanes and passing it to the region_of_interest() function provided. Lastly, a Hough Transform is applied on the masked image in order to detect the lane lines. These are then rendered onto the original image via the weighted_img() function.


In order to draw a single line on the left and right lanes, I modified the draw_lines() function to store line endpoints and separate them out by side: lines with negative slope belong to the left lane while positive slope lines belong to the right lane. The line values for both sides are then extrapolated using linear regression in order to draw a single line. To simplify this task and reduce code duplication I defined a helper function get_full_lane(). This method handles the linear regression computation as well as calculating the endpoints for the single lines by finding their equation. The bottom endpoint is calculated using the maximum y (image height) and solving for x. The upper endpoint is calculated using the maximum x value (left line) or the minimum x value (right line) and solving for y.


###2. Identify potential shortcomings with your current pipeline

The number of potential shortcomings could be considerable given the pipeline is heavily optimized for the sample project images.

One shortcoming would be processing images where lanes are wider (or narrower) than in the example images provided. The pipeline would not be able to detect lane lines falling outside the defined region of interest without further adjustments.

Other shortcomings include the presence of other elements which could be interpreted by the pipeline as lines, such as windshield wipers; or even old lane lines which have been painted over but can still be seen (and edge detected). 


###3. Suggest possible improvements to your pipeline

A possible improvement would be to better adjust the Hough Transform parameters so that a well defined region of interest isn't needed as much in order to detect lane lines, thus providing more flexibility with varying lane widths.

Another potential improvement could to add curve detection so that lane lines can be drawn over the entire road within view.
