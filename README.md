# LeNet-in-Pytorch
Many sources claim to have implemented LeNet, but have actually greatly simplified the algorithm. This project seeks to implement LeNet exactly as specified in "GradientBased Learning Applied to Document  Recognition" by LeCun et al. This is part of a bigger project which examines two papers fundamental to modern day computer vision techniques.

## Current progress
Currently, the complete model described by LeCun et al. has been implemented in pytorch (see results below) - except for a unique optimisation algorithm discussed in the paper as "stochastic diagonal levenburg-marquadt". This involves calculation of custom learning rate for each weight using the Hessian matrix (second derivatives of each weight). I may return to implementing this in the future.

# 


