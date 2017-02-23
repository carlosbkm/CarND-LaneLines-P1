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

1. Convert the image to grayscale for a simpler color selection.
2. Apply Gaussian Blur smoothing to get rid of noise.
3. Find the edges with canny edge detection.
4. Apply shape mask.
5. Apply hough transform to find line segments.
6. Separate lines in left and right and apply cuttoff filter based on slope to get rid of outliers (too horizontal lines)
7. Extrapolate the lines based on slope.
8. Draw the lines.

The main challenges I had to face on this development were on points 6 and 7. Extrapolate the lines is an action required to be able to draw a continous line with no gaps even when the line on the road is dashed.

To make this first I had to calculate slope on the segments, and based on this I separated them into left line and righ line group. Also, in the same process the segments with an angle out from a reasonable expected range are excluded. This is done to get rid of outliers, for example horizontal segments or any others likely to be non valid. All this is done in *separate_lines* method.

With the segments separated by left and right side of the lane, then we are ready to apply some algorithm which make us extrapolate the line from the segments found. Here, I tried several simple approaches:
* Mean slope: I calculate the mean slope from all segments, take a median segment and use that and the intersect points at the limits of the mask to draw a continous line. The code for this is at *master* branch
* Longest segment: I take the longest segment within its segments group (left or right) and use its slope and the intersect points to draw a continous line. The code is at *longest-line* branch.
* Minimum deviation: I calculate the deviation from the mean slope for every segment and then take the one with the minimum value. I use that segment and the intersect points to draw a continous line. The code is at *mininum-deviation* branch.

Most of the code is in *get_extrapolated_lines* and *draw_lines*. After checking the output, the three above gave all very similar results, so I took "Mean slope" as the winner. 

###2. Potential shortocomings

One potential shortcoming is what happen when there are very big gaps on the lines. This quite good solved with the extrapolation, but it could always represent a problem.

Another potential shortcoming is the light changes, shadows, tunnels, etc, which could drive to errors drawing the lines, as it happens sometimes with the challenge video.

Also, any vehicle too close to our camera, just in front of us would cause undesired results.

Another shortocoming would be when the road doesn't follow a straight line and we are at a corner or sharp curve.

And finally, with this approach the horizon line (ROI) is fixed, and the solution won't work well with images with a different set up.

###3. Suggest possible improvements to your pipeline

To minimize the errors for too big gaps on the lines, a better adjustment of the parameters at hough transform would definitely help. It would be necessary to double check the video, find the corner cases and try to tune it according to that.

For addressing the light and color problems, a tweaking of canny thresholds may cause an improvement. Also some transforms like adjusting contrast and gamma after grayscale would do a better job. And as a last resource, the image could be treat in its three different channels and after combined.

To extrapolate the lines, other more convenient methods could be used. For example, fit the line by using a linear regression.
