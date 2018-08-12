# CarND term1 project4
## Advanced lane lines
---

### Advanced Lane Finding Project

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/calibration.jpg
[image2]: ./output_images/perspective.jpg
[image3]: ./output_images/binary_show.png
[image4]: ./output_images/pipeline.png
[image5]: ./output_images/undist.jpg
[image6]: ./output_images/lane_finding.png
[image7]: ./output_images/result.png
[image8]: ./output_images/warped.jpg
[video1]: ./output_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first and second code cell of the IPython notebook located in "./code.ipynb".

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![][image1]
![][image5]

### Pipeline (single images)
#### 1. Provide an example of a distortion-corrected and perspective-transformed image.

In the previous part, I got the 'mtx' and 'dist' data of the camera, so here I can easily get undistorted image by using `cv2.undistort`.

To implement perspective transformation, I need to determine source and destination points. I plot the image of "straight_lines1.jpg" and get my source points like the picture shown below:
![][image2]
![][image8]
My final source and destination points are:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 572,470      | 260, 20        | 
| 275,670      | 260, 700      |
| 1030,670    | 1000, 700      |
| 711,470      | 1000, 20       |

Then I use `cv2.getPerspectiveTransform` and `cv2.warpPerspective` functions to show the raw picture in 'bird view'.

#### 2.  Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at 4th cell in code.ipynb). Here's examples of my outputs for this step. I made them in different color where green part is contributed by x_sobel, while blue part is contributed by combination of color transformation and magnitude sobel.

![][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

In this step, I combine the two steps above and use `test2.jpg` as an example:
![][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

This part codes are at 9th cell in `code.ipynb`. The lane detection part is in line 1 through 68, and the polynomial fit part is in line 105 through 131.

I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:
![][image6]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines 133 through 147 in my code in `code.ipynb`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step at the 10th cell in `code.ipynb`, where drawing green area part is located in lines 110 through 130, while printing information part is in line 133 through 155.  Here is an example of my result on a test image:

![][image7]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.

Here's a [link to my video result](./output_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The result shown by my video is not perfect, because it is still kind of unstable when the road changes material or there is shadows or the lane line is not that clear. I think there are two ways to make my pipeline more robust. First, use more soble threhold approaches and color transformation approaches to process images. More layers combines, more stable the result sould be. Second, now I use convolution algorithm to detect lane line pixels, however I think using window sliding approach may get better results.
