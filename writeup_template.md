## Writeup Template

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

1. Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
2. Apply a distortion correction to raw images.
3. Use color transforms, gradients, to create a thresholded binary image.
4. Apply a perspective transform to rectify binary image ("birds-eye view").
5. Detect lane pixels and fit to find the lane boundary.
6. Determine the curvature of the lane and vehicle position with respect to center.
7. Warp the detected lane boundaries back onto the original image.
8. Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image10]: ./output_images/undestorted_chessboard.png "Undistorted_chessboard"
[image1]: ./output_images/distorted_undistorted_chessboard_testImg.png "Undistorted"
[image2]: ./output_images/Undistorted_and_WarpedImage.png "Undistorted_and_WarpedImage"
[image3]: ./output_images/HLScolor_and_gradient_Image.png "HLScolor_and_gradient_Image"
[image4]: ./output_images/HLScolor_and_gradient_birdEye.png "HLScolor_and_gradient_birdEye"
[image5]: ./output_images/LaneLines_histogram_Image.png "LaneLines_histogram_Image"
[image6]: ./output_images/Sliding_Windows_Fit_PolynomialImg.png "Sliding_Windows_Fit_PolynomialImg"
[image7]: ./output_images/Lane_lines_with_offset.png "Lane_lines_with_offset"
[image8]: ./output_images/Measured_Curvature_in_meters.png.png "Measured_Curvature_in_meters.png"
[image9]: ./output_images/Draw_green_lane.png.png "Draw_green_lane.png"
[video1]: ./output_videos/project_video.mp4 "Video"


### 1. Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the second code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines.ipynb" 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image10]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

The code for this step is contained in the 4th code cell in the function `cal_undistort()` and the output is in the 6th code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines.ipynb"
I apply the distortion correction to one of the test images like this one:
![alt text][image1]

#### 2. Describe how you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps are contained in the 9th code cell in the function `color_gradient()` and the output is in the 10th code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines.ipynb").
I apply the distortion correction to one of the test images like this one:
![alt text][image3]

#### 3. Describe how you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, which appears in 7th code cell in the function `cal_undistort_unwarp()` and the output is in the 8th code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines.ipynb".  The `warper()` function takes as inputs an image (`img`), the source (`src`) and destination (`dst`) points are global variable so we can use the same v.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32(
    [[520, 508], 
    [770,508],
    [1030,687],
    [275,678]])
dst = np.float32(
    [[275,508],
    [1030,508],
    [1030,678],
    [275,678]])
```

This resulted in the following source and destination points:

| Source         | Destination   | 
|:--------------:|:-------------:| 
| 520,  508      | 275,  508     | 
| 770,  508      | 1030, 508     |
| 1030, 687      | 1030, 678     |
| 275,  678      | 275,678       |

![alt text][image2]

#### 4. Describe how you identified lane-line pixels and fit their positions with a polynomial?

Then I created a histogram of the image to show the lane lines positions like the following image, the histogram steps are contained in the 12th code cell in the function `hist()` and the output is in the 13th code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines.ipynb" 


![alt text][image5]

Then I use sliding window to fit my lane lines with a 2nd order polynomial kinda like the following image:
the sloding window steps are contained in the 14th code cell in the function `fit_polynomial()` and the output is in the 15th code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines.ipynb"
![alt text][image6]

I aso searched for the lane lines based on the prior detection with offset as the following.
the steps are contained in the 16th code cell in the function `search_around_poly()` and the output is in the 17th code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines.ipynb"
![alt text][image7]

#### 5. Describe how you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in the 18th code cell in the function `measure_curvature_real()`and the output is in the 20th code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines.ipynb"
![alt text][image8]

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in 21st code cell in the function `combine_img_with_green_lane()` and the output is in the 22nd code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines.ipynb".  Here is an example of my result on a test image:

![alt text][image9]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

I implemented this step in 24th code cell in the function `process_image()` of the IPython notebook located in "./CarND-Advanced-Lane-Lines.ipynb". to be called in process video step.
Here's a [link to my video result](./project_video.mp4)
