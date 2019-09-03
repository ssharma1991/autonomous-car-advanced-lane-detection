# Advanced Lane Finding for Self-Driving Cars

[//]: # (Image References)

[image1]: ./examples/Chessboard.png "Chessboard"
[image2]: ./examples/0_Original.png "Road Frame"
[image3]: ./examples/1_Undistorted.png "Undistorted"
[image4]: ./examples/2_Thresholded.png "Thresholding"
[image5]: ./examples/3_birds_eye.png "Prespective Transformation"
[image6]: ./examples/4_LaneDetectiom.png "Lane Detection"
[image7]: ./examples/5_Visualization.png "Visualization"
[video1]: ./test_videos_output/project_video_output.mp4 "Video"


## Pipeline
The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms and gradients to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

The complete pipeline is implemented in the function `pipeline()` which persorm all of the following steps in a sequential manner

---

## Step 1- Camera Calibration
Camera distortion is an intrinsic property of any image taken by a lens based camera. To get an image representing the actual environment, this distortion needs to be removed from the images first. This calibration process is carried out by calculating the **Camera Matrix** and the **Distortion Coefficients**. `CalibrateCamera()` function implements this process.

A set of chessboard images is used as input. The coordinates of chessboard corners are calculated and stored in `imgpoints` while their actual locations in real space are stored in `objpoints`. These are used to find camera calibration parameters and generate an undistorted image

![alt text][image1]

### Original to Undistorted
![alt text][image2]
![alt text][image3]

---

## Step 2- Binary Thresholding
Gradient and Color Thresholding is used to process the camera image. This process lane line data easier to detect and extract. `Thresholding()` function implements this process.

To carry out gradient thresholding, the sobel operator is used. A combination of both direction and magnitude thresholding is done for better results. Color Thresholding is done by converting RGB image to HLS image and isolating and thresholding the S-channel. Both are combined to generate the final binary image

![alt text][image4]

---

## Step 3- Prespective Transformation
Lane information is easier to process in a birds-eye view of the road. An appropriate prespective transformation is used to convert the binary image into top-down view of the road. Pixel data of reference points in a straight lane are used to create a trapezoid and transform it into a rectangle. `prespective()` function implements this process.

![alt text][image5]

---

## Step 4- Lane Pixel Detection and Curve Fitting
In this step, Left and right pixels grouped together. A histogram and tolerance window is used to approximately isolate lane pixels from surrounding noise. In a video, lane data from previous frames is be used to improve the efficiency of the process. Finally, a second order quadratic curve can be fitted to approximate the lane lines. Lane data is stored as instances of the `line` class. `fit_polynomial_new()` and  `search_around_poly()` functions are used to implement this process.

![alt text][image6]

---

## Step 5- Lane Curvature and Vehicle location within lane
Once the lane pixels are calculated, they can be mapped to real environment using standard lane guidlines set by the US Department of Transportation. The curve is used to calculate the curvature of lane and location of car within the lane. `measure_curvature_pixels()` and  `measure_location_in_lane()` functions are used to implement this process.

![alt text][image7]

---

## Step 6- Visualization
Finally, the lane data is ombined with camera view to visualize the workings of the algorithm. Curvature of lane and location of car within the lane are displayed as text on top of the image. `visualize()` function implements this process.

![alt text][image8]

---

## Results
A video captured by a camera installed in a car is processed and the algorithm for lane detection is run. The results have been shown in the video available [here][video1]


## Discussion
The lane detection by the current algorithm can be less accurate when shadows on road exist. Also, improvements need to be made to handle more challenging videos.




