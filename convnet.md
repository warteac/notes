### Convolutional Neural Networks (CNNs/ConvNets)

### 1. Concepts

- similar to ordinary neural networks made up of neorons that have learnable weights and biases.
- from the raw image pixels on one end to class scores at the other.
- loss function
- ConvNet receive images as inputs
### 2. Architecture
- Recall: Regular neural nets receive an input (a single vector) and transform it through a series of hidden layers. Each hidden layer is made up of a set of neurons, where each neuron is fully connected to all neurons in the previous layer, and where neurons in a single layer function completely independently and do not share any connections. The last fully-connected layer is called the "output layer" and in classification settings it represents the class scores.
- Regular Neural Nets don't scale well to full images.
- full connectivity is wasteful and the huge number of parameters would quickly lead to overfitting.
- the layers of a ConvNet have neurons arranged in 3 dimensions: width, height,depth
- the neurons in a layer will only be connected to a small region of the layer before it, instead of all of the neurons in a fully-connected manner.
- by the end of the ConvNet architecture we will reduce the full image into a single vector of class scores,arranged along the depth dimension.

> A ConvNet is made up of Layers. Every Layer has a simple API: it transforms an input 3d volume to an output 3d volume with some differentiable function that may or may not have parameters.

### 3. Conv layer
- the conv layer's parameters consist of a set of learnable filters. Every filter is small spatially (along width and height), but extends through the full depth of the input volume. ex: a first layer of a ConvNet might have size 5*5*3, 5 pixels width and height and 3 because images have depth 3, the color channels.
- During the forward pass, we convolve each filter across the width and height of the input volume and compute dot products between the entries of the filter and the input at any position. As we convolve the filter over the width and height of the input volume we will produce a 2-dimensional activation map that gives the responses of that filter at every spatial position.
- intuitively, the network will learn filters that activate when they see some type of visual feature such as an edge of some orientation or a blotch of some color on the first layer, or eventually entire honeycomb or wheel-like patterns on higher layers of the network.
- we will have an entire set of filters in each Conv layer,(e.g. 12 filters), and each of them will produce a separate 2-dimensional activation map. we will stack these activation maps along the depth dimension and produce the output volume.

#### local connectivity
- connect each neuron to only a local region of the input volume
- the spatial extent of this connectivity is a hyperparameter called the receptive field of the neuron(equivalently this is the filter size)
- the connections are local in space(along width and height), but always full along the entire depth of the input volume.

Example 1. For example, suppose that the input volume has size [32x32x3], (e.g. an RGB CIFAR-10 image). If the receptive field (or the filter size) is 5x5, then each neuron in the Conv Layer will have weights to a [5x5x3] region in the input volume, for a total of 5*5*3 = 75 weights (and +1 bias parameter). Notice that the extent of the connectivity along the depth axis must be 3, since this is the depth of the input volume.

Example 2. Suppose an input volume had size [16x16x20]. Then using an example receptive field size of 3x3, every neuron in the Conv Layer would now have a total of 3*3*20 = 180 connections to the input volume. Notice that, again, the connectivity is local in space (e.g. 3x3), but full along the input depth (20).

#### spatial arrangement
- output volume and how they are arranged
- three hyperparameters control the size of the output volume: the depth, stride and zero-padding.
- (1) depth: it corresponds to the number of filters we would like to use,each learning to look for something different in the input. (one filter can add one depth)depth column or fibre refer to a set of neurons that are all looking at the same region of the input 
- (2) stride 
- (3) zero-padding it will be convenient to pad the input volume with zeros around the border. nice feature to ensure that the input volume and output volume will have the same size spatially.

number of neurons: (W−F+2P)/S+1
W: input volume size
F: receptive field size
P: amount of zero padding
S: stride

#### parameter sharing
ex:
input images [227x227x3],on the first convolutional layer, it used F = 11, S = 4, P = 0. since (227-11)/4+1 = 55, and depth of K = 96, the conv layer output volume had size [55*55*96]. Each of the neurons in this volume are connected to the same [11*11*3] region of the input, but of course with different weights.

- in the ex above, we see that there are 55*55*96=290400 neurons in the first Conv Layer, and each has 11x11x3 = 363 weights and 1 bias. together, this adds up to 290400*363=105,705,600 parameters
- denoting a single 2-dimensional slice of depth as a depth slice (e.g. a volume of size [55x55x96] has 96 depth slices, each of size [55x55]), we going to constrain the neurons in each depth slice to use the same weights and bias.
- with this parameter sharing scheme, the first Conv Layer in our example would now have only 96 unique set of weights(one for each depth slice), for a total of 96x11x11x3=34848unique weights, + 96 biases.
- during backpropagation, every neuron in the volume will compute the gradient for its weights, but these gradients will be added up across each depth slice and only update a single set of weights per slice.

- if all neurons in a single depth slice are using the same weight vector, then the forward pass of the Conv layer can in each depth slice be computed as a convolution of the neuron's weights with the input volume (hence the name: Convolutional layer).
- it's common to refer to the sets of weights as a filter (or a kernel), that is convolved with the input

#### Convolution Demo


![](http://i.imgur.com/2mfOyWA.png)

Each element is computed by elementwise multiplying the highlight input(blue) with the filter (red), summing it up, and then offsetting the result by the bias.

#### implementation as matrix multiplication
1. if the input is [227x227x3] , and it is to be convolved with 11x11x3 filters at stride 4, then we would take [11x11x3] blocks of pixels in the input and stretch each block into a column vector of size 11x11x3= 363.
Iterating this process in the input at stride of 4 gives (227-11)/4+1=55 locations along with width and height, leading to an output matrix x_col of size [363x3025], where every column is a stretched out receptive field and there are 55x55=3025 of them in total.
2. the weights of the CONV layer are similarly stretched out into rows. For example, if there are 96 filters of size [11x11x3],this would give a matrix w_row of size [96x3015]
3. the result of convolution is now equivalent to performing one large matrix multiply np.dot(w_row, x_col).
4. the result must finally be reshaped back to its proper output dimension [55x55x96]
> this method can use a lot of memory, since some values in the input volume are replicated multiple times in x_col.
but the matrix multiply can be very efficient.

> ? why not stretching the input blocks into column vector and stretching the weights into row vector.?

#### backpropagation 
the backward pass for a convolution operation is also a convolution
#### 1x1 convolution 
in convnets, we operate over 3-dimensional volumes, and that the filters always extend through the full depth of the input volume. ex, if the input is [32x32x3], then doing 1x1 convolutions would efficiently be doing 3-dimensional dot products (since the input depth is 3 channels)
#### Dilated convolutions 
we can introduce one more hyperparameter called dilation. Filters can not be contiguous. it's possible to have spaces between each cells.

### 4. Pooling layer
- pooling layer is to progressively reduce the spatial size of the representation to reduce the amount of parameters and computation in the network, and hence to also control overfitting. 
- the pooling layer operates independently on every depth slice of the input and resizes it spatially, using the MAX operation.
- there are only two commonly seen variations of the max pooling layer found in practice: A pooling layer with F=3, S=2, (also called overlapping pooling) and more commonly F=2, S=2. Pooling sizes with larger receptive fields are too destructive.

#### General pooling
max pooling, average pooling, L2-norm pooling
![](http://i.imgur.com/z75Akk8.png)

#### back propagation
for the max pooling has a simple interpretation as only routing the gradient to the input that had the highest value in the forward pass. hence, during the forward pass of a pooling layer it is common to keep track of the index of the max activation(st also called the switches) so that gradient routing is efficient during backpropagation.

### 5. Normalization Layer

### 6. Fully-connected layer
Neurons in a fully connected layer have fully connections to all activations in the previous layer, as seen in regular neural networks. 
Their activations can hence be computed with a matrix multiplication followed by a bias offset.

### 7.Converting FC layers to CONV layers
difference between them: neurons in the conv layer are connected only to a local region in the input, and that many of the neurons in a conv colume share parameters.
common: both layers still compute dot products, so their functional form is identical.

### 8. layer sizing patterns

- input layer: should be divisible by 2 many times. common numbers include 32,64,96 or 224,384,512
- conv layer: should be using small filters (3x3 or most 5x5), using a stride of S = 1
- pooling layer: the most common setting is to use max-pooling with 2x2 receptive fields and with a stride of 2. note that this discards 75% of the activations in the input volume 

#### why use stride of 1 in conv
smaller strides work better in practice. stride 1 allow us to leave all spatial down-sampling to the pool layers.

#### why use padding?
if not, the information at the borders would be "washed away" too quickly.








