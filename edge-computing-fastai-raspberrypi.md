This is work in progress ... 


# Edge computing with fast.ai. Setting up fast.ai 1.0 with Python 3.7 and PyTorch 1.0 on a Raspberry Pi 3


## Update system and install required packages with apt
```
sudo apt update && sudo apt upgrade
sudo apt install git libssl-dev build-essential  libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libsqlite3-dev libbz2-dev libexpat1-dev liblzma-dev libffi-dev tk-dev uuid-dev cmake libopenblas-dev libatlas-dev libblas-dev m4 libjpeg-dev zlib1g-dev
```

## Install Python 3.7 
Details here https://gist.github.com/dschep/24aa61672a2092246eaca2824400d37f, https://stackoverflow.com/questions/27022373/python3-importerror-no-module-named-ctypes-when-using-value-from-module-mul, and https://bugs.python.org/issue30090

Compile and install 
```
wget https://www.python.org/ftp/python/3.7.1/Python-3.7.1.tgz
tar -xzvf Python-3.7.1.tgz
cd Python-3.7.1/
./configure
# optional: ./configure --enable-optimizations
make -j 4
sudo -H make altinstall
```
You can now run the newly installed Python with ```python3.7```. The old Python 2.7 and 3.5 that come with Raspbian or Ubuntu MATE are still installed and you can still call them with ```python``` and ```python3```.

Now create and activate a new Python 3.7 environment that we will use with PyTorch 
```
cd
python3.7 -m venv py37torch
source py37torch/bin/activate
```
While in the virtual enviromnent "(py37torch)" ```python``` and ```pip``` will run the binaries in the  ../py37pytorch/bin/ directory - which are from the newly installed Python 3.7: 

Update pip and install some requisites 
```
pip install --upgrade pip
pip install pyyaml pybind11 pgen cython wheel numpy
```

## Install PyTorch for CPU
Find more info here: https://gist.github.com/fgolemo/b973a3fa1aaa67ac61c480ae8440e754

The build for PyTorch will run out of memory if you do not increase the size of the swap file (=increase virtual memory size):

## For Raspbian only
Edit the swap config file with:
```
sudo vi /etc/dphys-swapfile
```
and change the CONF_SWAPSIZE to 2048:
```
CONF_SWAPSIZE=2048
``` 
restart the swap service:
```
sudo /etc/init.d/dphys-swapfile restart
```

## For Ubuntu MATE only
```
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```


Download and compile PyTorch. This will take quite some time ...
```
cd
source py37torch/bin/activate
git clone --recursive https://github.com/pytorch/pytorch

# Export two compilation flags in order to disable Nvidia/CUDA support and distributed computing. Othwise you will run into errors:
export NO_CUDA=1
export NO_DISTRIBUTED=1

cd pytorch
# optional - to use rc1: checkout tags/v1.0rc1 -b rc1-fastai
git submodule update --init
python setup.py install
```
... this will take a while

Install fast.ai v1.0
```
cd
source py37pytorch/bin/activate
pip install fastai
```
