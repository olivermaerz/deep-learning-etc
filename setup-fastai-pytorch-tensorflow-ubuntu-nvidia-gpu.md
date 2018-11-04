# Setup Ubuntu with multiple CUDA versions for pytorch, fastai, tensorflow on a machine with Nvidia GPU 

I have tested this setup on Ubuntu 18.04 LTS and 18.10 running on a local machine and on AWS EC2 instances (p2.xlarge instance with stock Ubuntu 18.04 image). 

## General setup

Update system and install nvidia drivers

```sudo apt install ubuntu-drivers-common
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update && sudo apt upgrade 
sudo ubuntu-drivers autoinstall
sudo reboot
```

Install Anaconda (replace version with latest available)
```
wget https://repo.continuum.io/archive/Anaconda2-5.3.0-Linux-x86_64.sh
bash Anaconda2-5.3.0-Linux-x86_64.sh
source .bashrc
```

## Setup fastai incl. pytorch

Create conda environment for fastai
```
conda create -n fastai
conda activate fastai
conda install -c pytorch pytorch-nightly cuda92
conda install -c fastai torchvision-nightly
conda install -c fastai fastai
```

Verify that pytorch (and fastai) is using GPU 
```
conda activate fastai
python 
>>> import torch
>>> torch.cuda.get_device_name(0)
``` 
Get and run the fastai course v3
```
git clone https://github.com/fastai/course-v3.git
cd course-v3/nbs/dl1/
jupyter notebook
```


## Setup tensorflow 

Create tensorflow environment
```
conda create -n tf
conda activate tf
conda install tensorflow-gpu
```

Verify that tensorflow is using GPU
```
conda activate tf
python
>>> import tensorflow as tf
>>> sess = tf.Session(config=tf.ConfigProto(log_device_placement=True)) 
```
