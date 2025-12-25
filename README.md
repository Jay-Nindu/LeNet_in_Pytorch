# LeNet-in-Pytorch
Many sources claim to have implemented LeNet, but have actually greatly simplified the algorithm. This project seeks to implement LeNet exactly as specified in "GradientBased Learning Applied to Document  Recognition" by LeCun et al. This is part of a bigger project which examines two papers fundamental to modern day computer vision techniques.

## Current progress
Currently, the complete model described by LeCun et al. has been implemented in pytorch (see results below) - except for a unique optimisation algorithm discussed in the paper as "stochastic diagonal levenburg-marquadt". This involves calculation of custom learning rate for each weight using the Hessian matrix (second derivatives of each weight). I may return to implementing this in the future.

## Results
I CANNOT TRAIN MY MODEL FULLY GOOGLE COLLAB RESTRICTS MY GPU CAPACITY

## Discussion of LeNet's unique architecture
![](LenetArchitecture.png)
The above figure has been taken directly from LeCunn et al.

## Convolutions
Convolutional layers use a significantly lower number of trainable parameters than fully connected layers. In a fully connected layer, all inputs are connected to all outputs. In computer vision this may be undesirable. While there are likely to be relevant associations between neighbouring pixels in an image, there is likely no relationship between pixels on different sides of an image. In a convolutional layer, each output is connected only to a subset of the inputs - where each of these inputs are neighbouring. This describes the receptive field.

## Subsampling
The layers S2 and S4 are pooling layers (similar to max pooling & average pooling layers). For these subsampling layers, each cell in in each feature map corresponds to four cells from the corresponding feature map in the previous layer. This receptive field of four-cells does not overlap with any other receptive field (in other words, stride = 2). The values of these four cells are added, multiplied by some trainable _weight_ parameter and added to trainable _bias_ parameter. This is then passed through the same hyperbolic tangent activaation function as the other layers (LeCunn et al. refer to this as a "squashing" function). 

## C3 - not fully connected
Normally with convolutions, each cell in the output is computed using every feature map in the prior layer. This is the case for all convolutional layers in LeNet except for C3.
C3 specifies which feature maps in S2 are to be taken to compute the feature maps for C3. See the below figure from LeCunn et al.

![](C2_connectivity.png)
Each outup of C3 uses every combination of 3 & 4 contiguous feature maps, every combination of 4 non-contiguous feature maps, and one outputthat takes utilises all 6 feature maps. LeCunn et al. say the reasons for this connectivity are 1) to reduce the overall number of connections, and 2) to "break symmetry" of the network by forcing each feature map of C3 to extract different details. 

## F6 - fully connected
For the second-to-last layer, all inputs from C5 are connected to all outputs in F6 (unlike the preceding convolutional layers).

## Radial basis function (RBF) layer
The final layer has 10 RBF units (one for each digit 0-9), each with 84 inputs. For each digit, there is a 7x12 bitmaps which have been designed to represent that digit (see the bitmaps below).

![](rbf_bitmaps.png)

A radial basis function computes the euclidean distance between the outputs of F6 and each bitmap - the closer the outputs are to the idealised bitmap, the smaller the output. 

## RBF loss function
