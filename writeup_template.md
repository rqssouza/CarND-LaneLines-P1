[//]: # (Image References)

[image1]: ./extra/open-cv-helper-gui.jpg

# **Finding Lane Lines on the Road**

**Finding Lane Lines on the Road**
The goals / steps of this project are the following:

- Make a pipeline that finds lane lines on the road

- Reflect on your work in a written report

---

# 1. Pipeline description

My pipeline consisted of 5 steps: Apply the Canny operator, apply geometric masks to the edge image,
find the average left and right lines from the masked edge image
and apply the lines to the original image. The last step consisted on find and tuning the used parameters
*filterSize*, *threshold1*, *threshold2*, *base*, *height*, *topCut*, *bottomCut*, *pointsThs*,
*minLineLength* and *maxLineGap*.
Each step is described below:

## 1.1 Apply the Canny operator

This step was done in the traditional way: First convert the image to gray scale then apply
the Gaussian filter and finally cv2.Canny operator.
The parameter used in this step were *filterSize*, *threshold1* and *threshold2*.

## 1.2 Apply geometric masks

For this step I used three combined masks: A triangle in the center of the image with size
*base* and *height*, a topCut mask with size *topCut*(measured from y=0) and a bottomCut mask with size *bottomCut*(measured from y=imageHeight).
I felt necessity to include the last two masks to try to complete the challenge
(mainly to cut out the hood of the car and the trees and guardrail ahead in the image)

## 1.3 Find the lanes

In this step first I applied the cv2.HoughLinesP with the parameters
*pointsThs*, *minLineLength* and *maxLineGap*. Then I separated the found lines in left lines and
right lines based on the slope. Then I sorted the lines and filtered out the ones with a slope difference
greater than 0.5 from the median line. Then took the mean left line and the mean right line as the
average lines. Finally I extended these lines until the end of the image.

## 1.4 Apply lanes

This step simply consisted on applying the lines found in the previous step to the original image.

## 1.5 Find the parameters

This step was to fine tune the parameters. For this task I developed a tool to help me find the parameters
interactively [opencv-gui-helper-tool](https://github.com/rqssouza/opencv-gui-helper-tool).
My tool was forked from [Maunesh's version](https://github.com/maunesh/opencv-gui-helper-tool)

![alt text][image1]

--

# 2. Potential shortcomings

One very nitid shortcoming is that I could not identify the yellow line on the challenge.mp4 video,
when the road changed from asphalt to concrete.

# 3. Possible improvements

I think this algorithm shall be considered like a 'naive' implementation and of course if I test it with
other real life videos several other shortcomings will appear. I think I have to do much more tests
to identify the new problems and maybe change to a more robust algorithm one like this one
[Precise line detection algorithm](https://www.hindawi.com/journals/js/2016/4058093/)
