## Writeup 

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.


## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points

---

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in section "HOG".

Hog for vehicle and non-vehicle can be visualized in that cell block.

#### 2. Explain how you settled on your final choice of HOG parameters.

I picked several parameter combinations and explored one by one using a subset of the data (approximately 20%). And the 8th parameter combinations seems best.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using combined features of HOG and color histogram. With the parameters I chose, I'm able to get close to 99% percent accuracy on test data.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?
I used the technique in lecture "Hog Sub-sampling Window Search". There is a specific code block dedicated to tuning the appropriate scales and positions and visualize the results.

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?
Please see the code block in this section. One thing I changed in the "block-norm" in hog function. Using "L2-Hys" as taught in lecture turned out to be a disaster. With that normalization, I get extremely poor results on image processing. I explored "L1", "L1-sqrt" and "L2" and settled on "L2" as it showed best performance.
Also, initially I had only HOG feature. Later I decided it might be better to also include color histogram feature. It improved the result. Also, setting a relatively higher threshold can filter out false positives.

---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_out.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

Heat map is applied as taught in lectures. Also, I added a memorization feature to record boxes in previous frames. With that, I set a relatively very high threshold to filter out any possible false positives. This is a tradeoff, as there are about 2 frames where the vehicle box was not shown. To me, eliminating false positives is more important.


---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Realizing that "L2-Hys" was the cause of my initial pipeline utter failure was an ordeal. It took me 10 hrs to find the cause where I suspected the least. Extremely painful process. Changing that to other three options works exponetially better. I admit I almost lost hope in the process.

The pipeline most likely to fail when vehicles are distant from camera, as the sliding windows most time cannot detect them. And if did, setting a high threshold would filter them out anyway. This is a tradeoff I made, as I think false positives are more important to filter out. Compared with that, detection of distant vehicles are most time not that important. To make it more robust, I probably need to tune window parameters and threshold a bit more. Also, if given more time, it would be better to come up with a more sophisticated mechanism for frame memorization.
