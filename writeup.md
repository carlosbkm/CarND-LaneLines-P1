#**Finding Lane Lines on the Road** 

##Writeup Template

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Description of the pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 8 steps:
1. Convert the image to grayscale for a more simple color selection.
2. Apply Gaussian Blur smoothing to get rid of noise.
3. Find the edges with canny edge detection.
4. Apply shape mask.
5. Apply hough transform to find line segments.
6. Separate lines in left and right and apply cuttoff filter based on slope to get rid of outliers (too horizontal lines)
7. Extrapolate the lines based on slope.
8. Draw the lines.


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
