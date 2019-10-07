## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

[image1]: ./imgs/color_transformation.png "COLOR TRANSFORM"
[image34]: ./imgs/image.png "CAMERA CALIBRATION"
[image2]: ./imgs/histogram_lane_lines.png "HISTOGRAM FINDINGS"
[image3]: ./imgs/unwarped.png "UNWARPED LINES"
[image4]: ./imgs/warped.png "WARPED LINES"
[image5]: ./imgs/lane_line.png "LANE LINES"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./output.mp4 "Video"


### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.
   I processed all of the camera calibration images and just added them to a list. I used findChessboardCorners to process my images
### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.
    The code for this step is contained in the first code cell of the IPython notebook located in "./examples/example.ipynb" (or in lines # through # of the file called `some_file.py`).  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image34]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

    Color transforms were performed with the color_transform function. I converted the image to hls format and then just focused on the saturation channel. Afterwards, I applied an x axis oriented sobel filter, normalized it, and then computed a binary based off of a threshold to get the following output:
    
![alt text][image1]
#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.
     I performed the birds eye transform with the warp function. I provided set parameters, used getPerspectiveTransform, and the output looks like this:
     
![alt text][image4]
#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?
    In find_lane_pixels and fit_polynomial, I calculated histograms by summing all of the pixels down to the bottom and then used then to approximate the bottm x positions of the lines. Afterwards I used the sliding window method to parse through all of the pixels, adding them to a list and then using fit polynomial to calculate the fitted lane lines.
    
    Afterwards, I used this information to calculate the margins.
#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.
    At measure curvature, I calculated the radius of curvature in radians; however, this calculation wasnt used in the project at all.
#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.
    No color, but yeah
![image6]
### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).
    The video wobbles a little bit when it encounters shadows, but other than that, the car seems to stay on the road.
![video1]
### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?
    The camera calibration doesn't seem to be very efficient and seems to work only sometimes. My pipeline would probably fail if the lighters were really bright that day or something. It would be helpful maybe to use different image channels such as a depth channel or something.