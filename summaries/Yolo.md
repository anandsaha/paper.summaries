# You Only Look Once, v1

* Link to paper: [http://arxiv.org/abs/1506.02640](**https://arxiv.org/abs/1409.4842**)

This summary is part of [this repository](https://github.com/anandsaha/paper.summaries).

----

### Introduction

Yolo is an object detection algorithm which performs the task in one pass of the image. Unlike other contemporary algorithms (e.g. RCNN family) which first generate region proposals and then run an image classifier on the proposed regions (and hence have multiple parts to it's wokflow), Yolo uses just one neural network to both predict bounding boxes and calculate class probabilities.

What makes Yolo fast (base model: 45 fps, fast model: 155 fps) and precise (63.4 mAP, not the best though) is the fact that it doesn't have multiple components to optimize. It lags in accuracy though, from other SoTA systems. It is a single monolithic neural network trained for bounding box and class predictions. Yolo treates object detection problem as a regression problem.

On the down side, Yolo makes localization errors compared to other systems.

### Big Picture

* Humans interprete images by looking at them once. We extract the salient features keeping a global perspective. Object detectors which split the image into parts (regions) lose this global perspective. 
* Yolo takes in an image and locates the objects and their class in one pass.
* Unlike sliding window and region proposal methods, Yolo looks at the entire image to encode the contextual information about classes. Yolo reasons globally about the image, hence makes less than half the number of background errors compared to Fast R-CNN
* Yolo learns generalizable representation of objects. That means if it is trained on dogs, it can do much better on paintings of dogs. Hence it understands the _concept_ of dog better.
* Yolo uses features from the entier image to predict each bounding box.


### The Algorithm

* First, Yolo divides the image into SxS (13x13)) grid. 
* Then, for each cell in the grid, it produces B (5) bounding boxes.
* Each bounding box is responsible for determining if it contains an object or not. And if it contains an object, it also spits out a confidence score as to how confident it is. Remember that at this point, it doesn't try to determine the class. Just that if it contains an object or not.
* Then, for each bounding box, Yolo calculates the class probabilities for each bounding box object. i.e. For each bounding box, we have 5 numbers:
    * The X and Y coordinate of the object (wrt to the cell it is in)
    * The height and width (wrt the entire image)
    * The confidence score (if it contains object or not)
* Also, for the grid cell, we predict C class probabilities (for the C classes we are predicting for)
* Hence, the goal is to predict if a cell is part of an object, and if so, which class. Creating N bounding boxes around the cell (and containing the cell) helps peak in the surrounding area (the global context) and then determine presence of an object.
* The score for each cell-boundingbox combination is given by: P(Class_i|Object) * P(Object) * IOU_truth_pred
* For evaluating on Pascal VOC, the parameters were: S = 7, B = 2 and C = 20. This resulted in a 7x7x30 tensor at the end of the model

### The Model

* A CNN architecture similar to GoogLeNet was used. The Inception modules were replaced with 1x1 reduction layers followed by 3x3 convolution layers.
* The CNN had 24 convolutional layers followed by 2 FC layers.
* Pretraining: The first 20 layers of the network (followed by an average pooling layer and FC layer) was trained on ImageNet for a week to gain an accuracy equalling GoogLeNet. Pretraining helps in improving performance.
* Once trained, it is converted to train for detection: 4 convolution layers and 2 FC layers are added with randomly initialized weights. Also, to train for detection, the image size is increased from 224x224 to 448x448
* The final layer predicts both the bounding box and the class. These values are normalized to fall between 0 and 1. Leaky ReLU is used as activation.

### The Loss function

The loss function is interesting. 

### Pros and Cons

* Very fast
* 

### Contrasting with other Algorithms

* R-CNN use region proposals and run classifier on the regions
    * Even after classification, post processing is required to tight fit the bounding boxes, eliminate duplicate detections and rescore the boxes based on other objects in the scene
    * Since there are many moving parts, they all need to be optimized to work well with each other
    * Since R-CNN and family splits the image, it looses global context, and fails to see bigger objects. It mistakes background patches as objects.
    * 
* DPM use sliding windows efficiently



**References**

* [This is a very good presentation on the subject](https://docs.google.com/presentation/d/1aeRvtKG21KHdD5lg6Hgyhx5rPq_ZOsGjG5rJ1HP7BbA/pub?start=false&loop=false&delayms=3000&slide=id.g137784ab86_4_1247)
* [Object detection with Yolo](http://machinethink.net/blog/object-detection-with-yolo/)
* [Bounding box object detectors](http://christopher5106.github.io/object/detectors/2017/08/10/bounding-box-object-detectors-understanding-yolo.html)