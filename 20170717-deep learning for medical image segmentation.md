Paper Review

### Title: 
Deep Learning for Medical Image Segmentation

### Cite:
Lai M. Deep learning for medical image segmentation[J]. arXiv preprint arXiv:1505.02000, 2015.


### 1. Abstract

This report provides an overview of the current state of the art deep learning architectures and
optimisation techniques, and uses the ADNI hippocampus MRI dataset as an example to compare
the effectiveness and effciency of different convolutional architectures on the task of patch-based 3-
dimensional hippocampal segmentation, which is important in the diagnosis of Alzheimer's Disease.
We found that a slightly unconventional "stacked 2D" approach provides much better classification
performance than simple 2D patches without requiring significantly more computational power. We
also examined the popular "tri-planar" approach used in some recently published studies, and found
that it provides much better results than the 2D approaches, but also with a moderate increase in
computational power requirement. Finally, we evaluated a full 3D convolutional architecture, and
found that it provides marginally better results than the tri-planar approach, but at the cost of a
very significant increase in computational power requirement.

### 2. Traditional Nnet
- action function
- gradient descent

### 3. why deep learning
- higher layers in deep networks can capture more abstract features
- vanishing gradient: layer-wise pre-training and rectified linear activation units
- dropout: max-pooling
- cnn

### 4. Hippocampus segmentation
- pre-processing 
- sampling: rectangle,mask
- Three methods:
- stacked 2d patches
 - three 24x24 patches,one around the voxel, one in parallel and above, and one in parallel and below
 - 3 layers: 20 5x5 kernels in first conv layer, 50 5x5 kernels in second conv layer, 1000 nodes fully-connected layer
 - no max-pooling layer

- tri-planar patches

 - 3 24x24 square pathes around the voxel, perpendicular to each axis
 - 2 convolutional layers (20 5x5 kernels  and 50 5x5 kernels) for each of the 3 pathes with no connections between them until the very end.
 - 1000 nodes fully-connected layer
 - no max-pooling

- 3d pathes
  - 24x24x24 patches
  - 20 5x5x5 kernels , 50 5x5x5 kernels and 1000 nodes fully-connected layer
  - no max-pooling layer

- post-processing: bobs
- image labeling
  > pathes are extracted for every voxel in the mask region, and the result is used to label the voxel. any voxel outside of the mask region is automatically classified as negtive.


### 5. conclusion
- see in abstract