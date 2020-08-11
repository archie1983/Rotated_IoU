# Differentiable Intersection Area of Oriented Boxes
## Introduction
This repo is an unofficial implementation of [IoU Loss for 2D/3D Object Detection](https://arxiv.org/pdf/1908.03851.pdf). It contains the Pytorch function which calculates the intersection area of oriented rectangles using GPU.

## Check List
- [x] Pytorch function to find intersection points of oriented rectangles
- [x] Pytorch function to check if corners of one rectangle lie in another 
- [x] CUDA extension to anti-clockwise sort vertices of the intersection polygon of two rectangles
- [x] Pytorch function to calculate the intersection of area of rectangles using functions above
- [x] Implementation of GIoU and DIoU for rotated rectangles (simplified) 
- [x] Test cases
- [x] Demo to validate the back-propagation
- [ ] Intersection of 3D bounding boxes
- [ ] Validate 2d/3d IoU loss in Object detection

## Requirements
Code is tested on Ubuntu 18.04. Following dependencies are needed

    cudatoolkit=10.2
    pytorch=1.5         # newer version should work as well
    numpy
    matplotlib
    argparse

## Usage

First, compile the CUDA extension.

    cd cuda_op
    python setup.py install

Then, run a demo which validate the Pytorch functions and CUDA extension.

    cd ..
    python demo.py

This demo trains a network which takes N set of box corners and predicts the `x, y, w, h` and `angle` of each rotated boxes. In order to do the back-prop, the predicted box parameters and the GT are converted to coordinates of box corners. The area of intersection is calculated using the Pytorch function with CUDA extension. Then, the GIoU loss or DIoU loss can be calculated. This demo first generates data and then do the training.

You are expected to see some information like followings:

    ... generating 819200 boxes, please wait...
    ... generating 81920 boxes, please wait...
    data saved in:  ./data
    [Epoch 1: 10/200] train loss: 1.1633  mean_iou: 0.1187
    [Epoch 1: 20/200] train loss: 0.9112  mean_iou: 0.2096
    [Epoch 1: 30/200] train loss: 0.8660  mean_iou: 0.1998
    [Epoch 1: 40/200] train loss: 0.8763  mean_iou: 0.2181
    [Epoch 1: 50/200] train loss: 0.8474  mean_iou: 0.2193
    [Epoch 1: 60/200] train loss: 0.9155  mean_iou: 0.2253
    [Epoch 1: 70/200] train loss: 0.8108  mean_iou: 0.2370
    [Epoch 1: 80/200] train loss: 0.7942  mean_iou: 0.2244
    [Epoch 1: 90/200] train loss: 0.7874  mean_iou: 0.2271

Note the `train loss` drops and the `mean_iou` increases, which shows the functions are differentiable.

## Acknowledgement
The idea of calculating intersection area is inspired by this paper:

    @INPROCEEDINGS{8886046,
        author={D. {Zhou} and J. {Fang} and X. {Song} and C. {Guan} and J. {Yin} and Y. {Dai} and R. {Yang}},
        booktitle={2019 International Conference on 3D Vision (3DV)}, 
        title={IoU Loss for 2D/3D Object Detection}, 
        year={2019},
        volume={},
        number={},
        pages={85-94},}

Some code for CUDA extension is modified from:

    @article{pytorchpointnet++,
        Author = {Erik Wijmans},
        Title = {Pointnet++ Pytorch},
        Journal = {https://github.com/erikwijmans/Pointnet2_PyTorch},
        Year = {2018}
    }
