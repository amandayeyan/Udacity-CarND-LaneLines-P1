# Udacity-CarND-LaneLines-P1
## Final Version

This version 3 works perfect for the challenge video. After my first submisison, I spent another day to redo the unit test. In the end, I found out there was something incorrect in the algorithm pipeline. I deleted bitwise_and between the color mask and canny edges since this step incorrectly washed out non-overlapping but true lane lines. I deleted creating histogram method as it did not help much, which I previously thought as an automatic way to compute the most two prominent slopes for the left lane and right lane.

Other than that, I defined yellow mask in hsv colorspace but white mask in rgb colorspace. It turns out the white mask defined in rgb colorspace performs much better result than that defined in hsv colorspace.

Further, by tuning the Hough line parameters threshold, min_line_length, max_line_gap as suggested by the reviewer, eventually my algorithm can correctly detect lane lines in the challenge video.

To make the pipeline more complete, I considered some fatal scenario such as no Hough line detected at all. In this case, we need conditional statement, only when Hough lines detected, then it proceeds to line fitting, otherwise it skips the remaining steps. However, if we process video rather than a single image, we can actually retain the lane information of previous frame to draw detected lane lines on the present frame.
## The new algorithm pipeline works as follows.
1. read image
2. convert to grayscale
3. smooth the original image with a Gaussian filter (kernel size = 5)
4. run color mask on the smoothed color image, and the result is a binary image<br /> 
      4.1 define yellow mask, white mask and bitwise_or to combine as color mask.

5. detect canny edges on the color mask image, retain only the canny edges in a defined polygon region<br /> 
      5.1 define a polygon region<br /> 
      5.2 bitwise_and the original canny edges with this polygon region
      
6. run Hough transform to find line segments (A line segment can be mathematically represented by y=mx+b)<br /> 
      6.1 if no line segments are detected, then the algorithm just skip the the remaining steps, no line fitting or drawing lines will be executed.

7. run linear line fitting to compute slope m and intercept b<br /> 
      7.1 compute the start and end points of each lane, given that both points sit at the boundary of the defined polygon.

8. draw lane lines on the original color image


## Lesson Learned

Be patient and must do unit test!
