# Deep CNN Feature by MXNet

## Requirements

### Environments

* Ubuntu 16.04
* CUDA 9.0
* python 2.7
* opencv-python
* mxnet-cu90 
* numpy

We used virtualenv to setup a deep learning workspace that supports MXNet. Run the following script to install the required packages.
```
virtualenv --system-site-packages ~/cnn_feat
source ~/cnn_feat/bin/activate
pip install -r requirements.txt
deactivate
```

### Required models

Run `do_prepare.sh` to download pre-trained CNN models.

```
# Download resnet-152 model pre-trained on imagenet-11k
./do_prepare.sh
```

## Get started

### Data

Store videos into `VideoData` under collection folder and store images into `ImageData` if you have extracted frames from videos. Videos with `.mp4 .avi .webm` extention and gif are supported. Images should have `.jpg` extention.

### Step 1. Extract frames from videos

```
collection=toydata
source ~/cnn_feat/bin/activate
./do_extract_frames.sh $collection
```

### Step 2. Extract frame-level CNN features

```
collection=toydata
source ~/cnn_feat/bin/activate
./do_resnet152-11k.sh $collection
./do_resnext101.sh $collection
```

### Step 3. Obtain video-level CNN features
```
collection=toydata
source ~/cnn_feat/bin/activate
./do_feature_pooling $collection pyresnet-152_imagenet11k,flatten0_output,os
./do_feature_pooling $collection pyresnext-101_rbps13k,flatten0_output,os
```

### Step 4. Feature concatenation
```
collection=toydata
source ~/cnn_feat/bin/activate
featname=pyresnext-101_rbps13k,flatten0_output,os+pyresnet-152_imagenet11k,flatten0_output,os
./do_concat_features.sh $collection $featname
```
