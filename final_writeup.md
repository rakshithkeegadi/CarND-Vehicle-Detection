# **[Vehicle Detection Project](https://github.com/rakshithkeegadi/CarND-Vehicle-Detection/blob/master/vehicle_detection.ipynb)** 
by [Rakshith Krishnamurthy](https://www.linkedin.com/in/rakshith-krishnamurthy-360682b/)

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/380_480.png "380_480"
[image2]: ./output_images/420_620.png "420_620"
[image3]: ./output_images/440_640.png "440_640"
[image4]: ./output_images/460_660.png "460_660"
[image5]: ./output_images/480_680.png "480_680"
[image6]: ./output_images/Car_Hog_image.png "Car_Hog_image"
[image7]: ./output_images/cmap_jet.png "cmap_jet"
[image8]: ./output_images/heatmap_gray.png "heatmap_gray"
[image9]: ./output_images/Non_car.png "Non_car"
[image10]: ./output_images/car_rectangles.png "car_rectangles"
[image11]: ./output_images/final_car_image.png "final_car_image"

Car_Hog_image.png	Commiting the images for writeup	7 hours ago
Non_car.png	commit more image	4 minutes ago
car_rectangles.png	commit more image	4 minutes ago
cmap_jet.png	Commiting the images for writeup	7 hours ago
heatmap_gray.png

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The HOG features are extracted by using the function get_hog_features() and this enables the classification of the imagee better. The way the hog features behave for a car and non car images is as shown as below

#### Car image
![alt text][image6]

#### Non car image
![alt text][image9]

The hog features give us a clear understanding of a car images is different from a non car image. This sets us up to train our model so that when we run our video the model can identify car and non car images. 

#### 2. Explain how you settled on your final choice of HOG parameters.

A lot of combinations were tried first starting off different color spaces to clasiffy the images and the best one turned out to be YCrCb and also choosing 'ALL' the hog channels made the classification better.


#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

The classifier parameters chosen to train the model is as mentioned below. 

| Parameters    | Values | 
|:-------------:|:------:| 
| colorspace    | YCrCb  | 
| orient        | 9      |
| pix_per_cell  | 8      |
| cell_per_bloc | 2      |
| hog_channel   | ALL    | 
| spatial_size  | (16,16)|
| hist_bins     | 16     |

We are training by choosing a data set which has almost equal sets of car and non car images. This is chosen to avoid overfitting. 

The car images and non car images features are extracted and then they were randomly shuffled so that the classifier does not memorize the images in sequential order. SVC Linear classifier was used to train and fit the model. Used 20 percent of the car and non car samples to split it as testing samples after training. Once the training was done the test samples were used to check the accuracy. The accuracy turned out to be 98.48%. 



### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

The sliding window was used to search the differnt parts of the image to read samples of images and the identify if they are car and non car images. The sliding winodw uses a small part of the complete image and checks if these images are car images. Here, the area of interest is the lower half of the images because the cars are not found on the upper half of the window. 


#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

The following is the output of the pipline images. To predict the car images better I had to choose different scales for overlapping Y axis areas of interest. The challenging part was to identify the car when it was slight further away and make sure I had enough section of the image to identify it. Initially I used LUV and obatained an accuracy of 93% and then later changed to YCrCb and this improved the performace to 98%.

By trial and error method I chose to choose 6 differnt scales (1,1,25,1,5,2,2,5) for different area of interest.
Here are few examples of the scaled images.

![alt text][image1]

![alt text][image2]

![alt text][image3]

![alt text][image4]

![alt text][image5]



Once the scaling is done then we used to identify the multiple sliding windows in which we identify cars like the images shown below 

![alt text][image10]

After that multiple rectangles are done then we use a heat map in the image to identify the region of the car in the image.

![alt text][image8]

Once the heat images are found we then go ahead and then draw the rectangle as shown in the image below so that this can be  used as a tempelate to run it on videos.
![alt text][image11]


### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

The pipeline was created very similar to the images but one extra thing I had to do was to identify the region of images and multiple sliding window scales used to gather images of the cars. The scales vary from 1 to 2.5 which does well in identifying the car.

The other extra thing I had to do with my images was to have a class and extract the most recent frames of data to draw the rectangles from the class variables. I used the recent most 5 rectangles from the video frames to identify the cars and hence this helped me get rid of most of the false positives.

The code for the pipeline can be found under the pipeline section.


---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

1. More data is required to make analysis of the car images. This might not identify the cars of all types
2. The whole video and vehicle mapping is done in day light and care must be taken to impelent it in dark light
3. It considers only cars we might see trucks and busses so other sets of images might be required.

