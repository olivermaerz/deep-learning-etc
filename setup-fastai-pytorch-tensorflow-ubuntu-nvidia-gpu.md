# Setup Ubuntu with multiple CUDA versions for PyTorch, fast.ai, TensorFlow on a machine with Nvidia GPU 

I have tested this setup on Ubuntu 18.04 LTS and 18.10 running both on a local machine and on a AWS EC2 instance (p2.xlarge instance with stock Ubuntu 18.04 image). This install will take care of drivers and all. There is no need to separately install CUDA or CUDNN as Anaconda will install the required versions for each conda enviroment below. 

## General setup

Update the system and install the latest Nvidia drivers

```sudo apt install ubuntu-drivers-common
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update && sudo apt upgrade 
sudo apt install ubuntu-drivers-common
sudo ubuntu-drivers autoinstall
sudo reboot
```

Install Anaconda (replace the filename below with the latest version available - check: https://www.anaconda.com/download/#linux)
```
wget https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh
bash Anaconda3-2019.03-Linux-x86_64.sh
source .bashrc
```

## Setup fast.ai (incl. PyTorch, CUDA, and CUDNN)

Create a conda environment for fast.ai and PyTorch for Nvidia CPU 
``` 
conda create -n fastai python=3.7
conda activate fastai
conda install -c pytorch -c fastai fastai
conda install jupyterlab
```

Verify that PyTorch (and therefore fast.ai) is really using the GPU 
```
conda activate fastai 
python 
>>> import torch
>>> torch.cuda.get_device_name(0)
``` 

Optionally create environment for fast.ai and pytorch using CPU only
``` 
conda create -n fastai python=3.7
conda activate fastai
conda install -c pytorch pytorch-cpu torchvision
conda install -c fastai fastai
conda install jupyterlab
```

Download fast.ai course v3 and run Jupyter Notebook
```
sudo apt install git
git clone https://github.com/fastai/course-v3.git
cd course-v3/nbs/dl1/
conda activate fastai
jupyter lab
```
Note: Tunnel port 8888 from the jupyter notebook or lab through ssh with `ssh -L 8888:localhost:8888 ubuntu@<hostname>.<region>.compute.amazonaws.com`


## Setup TensorFlow (incl. CUDA and CUDNN)

Create a TensorFlow environment and install the TensorFlow version that runs on the GPU
```
conda create -n tf python=3.7
conda activate tf
conda install tensorflow-gpu pandas matplotlib jupyterlab notebook scipy scikit-learn opencv
```

Verify that TensorFlow is really using the GPU
```
conda activate tf
python
>>> import tensorflow as tf
>>> sess = tf.Session(config=tf.ConfigProto(log_device_placement=True)) 
```

Optionally create TensorFlow environment that uses CPU only
```
conda create -n tf-cpu python=3.7
conda activate tf-cpu
conda install tensorflow pandas matplotlib jupyterlab notebook scipy scikit-learn opencv
```

