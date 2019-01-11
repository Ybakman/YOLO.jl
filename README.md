# YOLO.jl - Work in progress!
Implementation of YOLO Object Detection models in Julia
Based on https://pjreddie.com/darknet/yolo/

## Downloading a dataset
Datasets are not included in this package due to their size, but can be downloaded using the provided download scripts.

Coco (~21 GB of zip files):

`include("<pkg root>/datasets/coco/get_coco_dataset.jl")`


## Initialize with a backend (Flux supported. Knet will be added)
```
using Flux
using YOLO
YOLO.LoadBackendHandlers()
```

## Load a training model
```
YOLOdir = dirname(dirname(Base.pathof(YOLO))); #Run dirname twice to get to package root from /src
m = include(joinpath(YOLOdir,"models/yolov2-tiny/trainingmodel.jl"))
```

## Load and prepare an image
```
using Images
imfile = string(@__DIR__,"examples/COCO_train2014_000000000650.jpg")
im, img_size, img_originalsize, padding = YOLO.loadprepareimage(imfile,(416,416)) #Loads, pads and resizes image
im_input = Array{Float32}(undef,416,416,3,1)
im_input[:,:,:,1] = permutedims(collect(channelview(im)),[3,2,1]);
@show typeof(im_input) size(im_input)
numClasses = 20
```

## Run model
```
output = m(im_input)
summary(output)
```

## Run a pretrained model


## Working with datasets

## Train a model
