# OverFeat: Integrated Recognition, Localization and Detection using Convolutional Networks

* Link to paper: [https://arxiv.org/abs/1312.6229](**https://arxiv.org/abs/1312.6229**)

This summary is part of [this repository](https://github.com/anandsaha/paper.summaries).

----

* A unified model for image classification, object locatization and detection. The paper claims that image classification and image detection accuracy increases if you train the model for both the tasks simultaneously.

Three novel ideas:

* Apply a ConvNet at multiple locations in the image, in a sliding window fashion, and over multiple scales. Even with this, however, many viewing windows may contain a perfectly identifiable portion of the object (say, the head of a dog), but not the entire object, nor even the center of the object. This leads to decent classification but poor localization and detection.
* Thus, the second idea is to train the system to not only produce a distribution over categories for each window, but also to produce a prediction of the location and size of the bounding box containing the object relative to the window.
* The third idea is to accumulate the evidence for each category at each location and size.

* Five guesses are allowed to find the correct answer (this is because images can also contain multiple unlabeled objects).

For Classification:

* Overfeat used AlexNet. Was trained on 1.2 million images, with 1000 classes.
* Uses fixed input size of images during training, but milti-scale classification during inference.
* Images are downsampled to 256 pixels. 5 random crops and their horizontal flips of 221x221 are extracted and presented to the network in mini batches of 128.
* Weights initialization, momentum, learning rate decay, weight decay and dropouts are used.
* Layers 1 to 5 are like AlexNet except: a) No contrast normalization is used b) pooling regions are non overlapping c) the model has larger 1st and 2nd layer feature maps due to smaller strides. Larger strides are good for speed for hurts accuracy.
* Multi Scale Classification: 

Feature Extractor:

* The paper calls its feature extractor OverFeat - there is a Fast one and an Accurate one. The Accurate one has double the parameters than Fast one.

* Implements sliding window algorithm as a set of convolutions, thereby reducing computation. The FC layers are replaced by convolution layers.

TODO (Not complete)

References:

* http://vision.stanford.edu/teaching/cs231b_spring1415/slides/overfeat_eric.pdf
* http://courses.cs.tau.ac.il/Caffe_workshop/Bootcamp/pdf_lectures/Lecture%206%20CNN%20-%20detection.pdf
