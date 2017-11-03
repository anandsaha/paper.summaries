# Going deeper with convolutions

* Link to paper: [https://arxiv.org/abs/1409.4842](https://arxiv.org/abs/1409.4842)
* Paper Published: Sep 2014

This summary is part of [this repository](https://github.com/anandsaha/paper.summaries).

----

## Summary 

This paper started the Inception lineage. This won the 2014 ImageNet challenge. This paper focused on making effecient use of computing resources while doing inference, so that it is usable in constrained environments like mobiles and embedded systems. The paper notes that recent advancements in computer vision was made possible due to "new ideas, algorithms and improved architecture" as opposed to "more powerful hardware, larger datasets and bigger models".

Traditional CNNs:
- Stacked fully connected conv layers
- 
- 

The way to increase accuracy is to go deeper and wider. But:
- increases memory and computation (linear increase in filters lead to quadradic increase in compute)
- needs more data
- 

GoogLeNet
- 
12x less parameters than AlexNet. Lower memory use, lower power use. More accurate than AlexNet. 1.5 billion add/mul;tiply.
- 2x less computation wrt AlexNet

Inspiration


### Going deeper





https://www.youtube.com/watch?v=ySrj_G5gHWI
