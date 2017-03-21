#**Finding Lane Lines on the Road** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the 

* Reflect on your work in a written report



### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps. 
    1. Select yellow and white pieces from the original RGB image, this method is noise resistent
    white_yellow_image = select_rgb_white_yellow(img)
    2. Convert to gray scale
    gray_image = grayscale(white_yellow_image)
    3. Apply Gaussian Blur
    blurred_image = gaussian_blur(gray_image)
    4. Use Canny edge detection
    edge_image = canny(blurred_image)
    5. Crop the edges using a Region of Interest
    roi_image = roi(edge_image)
    6. Use Hough transform to get the candidate lane lines
    list_of_lines = hough_lines(roi_image)
    7. Extrapolate the left/right line of the current lane
    lane_image = draw_lines(img,lane_lines(img,list_of_lines))

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:
    1. Pick out those lines satisfying certain angle, and depending on whether they are on the left side or right side of the ROI region, categorize them to left_lines and right_lines list respectively
    2. Use OpenCV cv2.fitLine method to get slope and intercept 
    3. Use the left slope & intercept and right slope & intercept, and ytop ybtm of the ROI region to derive each of the two end points of the extrapolate left lane line and right lane line

If you'd like to include images to show how the pipeline works, here is how to include an image: 



###2. Identify potential shortcomings with your current pipeline

During the period of P1 project, I'm thinking a lot while I'm driving by myself.

One potential shortcoming would be what would happen when it's not on a highway, but in urban region how to detect lanes. In this case, right lane lines are often curbs, how to detect them? 

Another shortcoming is (this comes into my mind when I watch the video of a Raspberry PI robot car lane tracking demo) how to make the lane tracking still effective when there's a sharp turn on the road. Not sure if current statically defined candidate angle identification method for lane still work.

Another shortcoming could be how to be noise resistent. It's not tested yet. Because I'm not quite sure, what a "noise" lane mean. It could be that there're some construction stuffs near the lane, or could be some crossing lines on the lane. Not sure if my algorithm is still strong enough to idenfy the correct lane lines.


###3. Suggest possible improvements to your pipeline

A possible improvement would be to improve the performance. Currently I don't have time to do profiling work.

Another potential improvement could be to try to be adaptive to sharp turn.