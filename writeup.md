# **Finding Lane Lines on the Road** 

## Project 1 Writeup by Attila Kesmarki

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Code and data

All the codes of the algorithm, the test image and test video processing, plus the output data are in my Project 1 jupyter notebook folder:
* Code: P1.ipynb
* Output images: test_images_output/ folder
* Output videos: test_videos_output/ folder
* And this report with the attachments: writeup.md and the images in the pics/ folder 

---

### Reflection

### Lane detection pipeline

My pipeline which recognizes lanes in front of the car consists of 5 steps.
To demonstrate these processing phases, I will show the internal resulting images after each step. 
#### Input: This example image was recorded by the camera on the top of the car: 
![alt text][image1]

#### 1. First, I convert the input image to grayscale, because the following Canny algorithm needs pixel magnitudes (intensity values) to detect the gradients. See:
![alt text][image2]


#### 2. In the next step, a gaussian blur algorithm is executed on the image to remove the noise, the too small features from the image.  
![alt text][image3]

#### 3. The third processing step is the Canny edge detection algorithm which detects the contours in the image. It can be seen that it correctly recognizes the lane markings: 
![alt text][image4]

#### 4. Then I clear the nonimportant parts in the image, because I only need to focus on a trapezoid area in front of the car: 
![alt text][image5]

#### 5. The last processing is to detect the lanes itselves. This step in the pipeline consists of 4 parts:
#### 5.1: First, I use the Hough line transformation algorithm to detect straight lines in the previous contour image.
#### 5.2: Then I group them by steepness: Lines with similar slope values get into the same "bucket". These lines are not directly stored in the bucket, but their representative constituent points are getting into the buckets, with a predefined density. Because of this, the number of points going into the bucket depends on the length on the lines.  
#### 5.3: After processing all lines, at most 2 buckets with the most number of points in them will be selected, and the other buckets will be dropped. 
#### 5.4: For each selected bucket's points, a fitLine algorithm (using simple euclidean distance) calculates a line, which is the detected lane. I draw it with a red stipe after clipping it into the lower 40% of the screen like this: 
![alt text][image6]

#### Output: And, the result rendered onto the original image: 
![alt text][image7]

Other than this simple example image above, you can check out the 2 videos in the test_videos_output folder. The third video, challenge.mp4 only highlights the shortcomings of this relatively simle pipeline. 

### 2. Identify potential shortcomings with your current pipeline
* First of all, there is a hug problem with the algorithm: it doesn't handle curving lanes at all, it just wants to fit a straight line on the lane drawings.
* Other than that, it's very sensitive to the road surface qualify, and to the frequently changing light/shadow sections.
* In case of video: The algorithm does not utilize the knowledge learnt from the past previous frames. Even in a fast car, the camera-recorded frames are no completely independent of each other. IMHO this could, and should be used.


### 3. Suggest possible improvements to your pipeline

* A possible improvement would be to use a specific color channel, or a combination of color channels, and not just the simple grayscale one. For example R can be pretty sensitive to sunshine, B is not so much... 
* All the sample images were taken during optimal weather conditions. The algorithm could have been better tuned if other images were provided with different weather conditions.  

---

[//]: # (Image References)

[image1]: ./pics/orig.jpg "Input of the pipeline: The original image"
[image2]: ./pics/gray.jpg "Step1: convert it to grayscale"
[image3]: ./pics/blurred.jpg "Step2: blurred image"
[image4]: ./pics/canny.jpg "Step3: after applying the canny edge detection algorithm"
[image5]: ./pics/masked.jpg "Step4: only work on the interesting part"
[image6]: ./pics/hough.jpg "Step5: identify and fit the lanes"
[image7]: ./pics/combo.jpg "Step6: the result displayed on the original image"
