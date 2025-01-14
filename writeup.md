# **Traffic Sign Recognition**

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./output/visualization.jpg "Visualization"
[image2]: ./output/grayscale.jpg "Grayscaling"

[image4]: ./Traffic_signs_test/4.jpg "Traffic Sign 1"
[image5]: ./Traffic_signs_test/35.jpg "Traffic Sign 2"
[image6]: ./Traffic_signs_test/38.jpg "Traffic Sign 3"
[image7]: ./Traffic_signs_test/14.jpeg "Traffic Sign 4"
[image8]: ./Traffic_signs_test/15.jpg "Traffic Sign 5"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799.
* The size of the validation set is 4410.
* The size of test set is 12630.
* The shape of a traffic sign image is (32,32,3).
* The number of unique classes/labels in the data set is 43.

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the training data set. It is a bar chart showing how the data distribute.

![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

First,I decided to convert the images to grayscale because the color affect little for the recognition of traffic signs,and it can reduce the training features.

Here is an example of a traffic sign image before and after grayscaling.

![alt text][image2]

Second,I normalized the image data,so that the data has mean zero and equal variance.

Third,I shuffled the training data.If not,A neural network might see the same type of sign hundreds of times in a row. That will lead to a bad weight of the model.

#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					|
|:---------------------:|:---------------------------------------------:|
| Input         		| 32x32x1 gray image   							|
| Convolution 3x3     	| 1x1 stride, valid padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x6 				|
| Convolution 3x3	    | 1x1 stride, valid padding, outputs 10x10x16  |  
|RELU|                                                                      |
| 	Max pooling|         2x2 stride,  outputs 5x5x16       |
|Flatten |  output 400|
| Fully connected		| output 120   	|
|RELU|      |
|Dropout|     keep_prob 0.75|
|      Fully connected   |    output 84           |
|        RELUE    |                    |
|         Dropoout         |          keep_prob  0.75      |
|         Fully connect   |          output 43    |
| Softmax				|      				|
|	cross_entropy		|         |
|	loss_operation		|						|
|    Optimizer     |             AdamOptimizer            |  


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used:
Optimizer : AdamOptimizer
learning rate :0.001
epochs : 20
batch size: 60

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 98.6%
* validation set accuracy of 93.1%
* test set accuracy of 90.9%

1. I used the LeNet architecture. It is made up of several convolution layers, Pooling layers, and Fully connect layers. It can extract the features of the image very well.
2. After using it, you will find that the accuracy of training set is very high, the accuracy of validation set is a little low.That's because, there is a overfitting on the training data.
3.  I took a dropout method on the fully connect layers to solve the problem.
4. At first, I put the RGB images into the net, the accuracy can get about  88%. In order to improve the accuracy and reduce the training time, I turn the images into gray and make them normalized.


### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image4] ![alt text][image5] ![alt text][image6]
![alt text][image7] ![alt text][image8]

The first image might be difficult to classify because there are many similar shapes on different speed.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					|
|:---------------------:|:---------------------------------------------:|
|   70 km/h            |             Ahead only              |
| Ahead only      		|            Ahead only  						|
| Keep right    			| Keep right 									|
| Stop     			  	| Yield											|
| No vehicles      		| No vehicles			 				|

The model was able to correctly guess 3 of the 5 traffic signs, which gives an accuracy of 60%.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)
For the first image, the model is not very well, the top five images dose not contain it. It only learned a circle. The number 70 isn't learned. The top five soft max probabilities were

| Probability         	|    Prediction	        					|
|:---------------------:|:---------------------------------------------:|
| 91%       			| Ahead only 									|
| 8%     				| Road work										|
| 0				        | Traffic signals										|
| 0	      			    | Yield			 				|
| 0			          | Turn left ahead   							|


For the second image, the model is very robust. It have a 100% accuracy.

| Probability         	|     Prediction	        					|
|:---------------------:|:---------------------------------------------:|
| 100%       			| Ahead only  									|
| 0       			    	| Yield									|
| 0				        | Road work											|
| 0	      			    | Priority road			 				|
| 0			          | Bumpy road   							|

For the third image, the model is very robust. It have a 100% accuracy.

| Probability         	|     Prediction	        					|
|:---------------------:|:---------------------------------------------:|
| 100%       			| Keep right 									|
| 0       			    	| Yield									|
| 0				        | Turn left ahead										|
| 0	      			    | Priority road			 				|
| 0			             | Road work							|

For the forth image, the model is very bad. There is no SOPE sign in the top images.

| Probability         	|     Prediction	        					|
|:---------------------:|:---------------------------------------------:|
| 53%       			| Yield								|
| 29%       	    	| Priority road									|
| 11%				        | No passing									|
| 5%	      			    | Ahead only		 				|
| 1%			          | Turn left ahead  							|

For the fifth image,  the model is very robust. It have a 100% accuracy.

| Probability         	|     Prediction	        					|
|:---------------------:|:---------------------------------------------:|
| 100%       			| No vehicles									|
| 0       			    	| No passing									|
| 0				        | Priority road										|
| 0	      			    | Roundabout mandatory				 				|
| 0			           |  Speed limit (100km/h)						|
### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?
