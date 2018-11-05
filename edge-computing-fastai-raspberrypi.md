This is work in progress ... 


# Edge computing with fast.ai v1. Setting up fast.ai with Python 3.7 and PyTorch on a Raspberry Pi 3


## Install Python 3.7 
Details here https://gist.github.com/dschep/24aa61672a2092246eaca2824400d37f plus here https://stackoverflow.com/questions/27022373/python3-importerror-no-module-named-ctypes-when-using-value-from-module-mul

Install some required packages with apt
```
sudo apt-get install build-essential tk-dev libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libsqlite3-dev libssl-dev libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev python-dev python-setuptools python-pip python-smbus libffi-dev git 
sudo apt-get install libopenblas-dev cython libatlas-dev m4 libblas-dev cmake python3-yaml
```

Compile and install 
```
wget https://www.python.org/ftp/python/3.7.1/Python-3.7.1.tgz
tar -xzvf Python-3.7.1.tgz
cd Python-3.7.1/
./configure --enable-optimazations
make -j 4
sudo make altinstall
```

Update pip and install pyyaml
```
pip3.7 install --user --upgrade pip
pip3.7 install --user pyyaml
```

## Install PyTorch for CPU

Details here: https://gist.github.com/fgolemo/b973a3fa1aaa67ac61c480ae8440e754

Export two compilation flags in order to disable Nvidia/CUDA support and distributed computing
```
export NO_CUDA=1
export NO_DISTRIBUTED=1
```
Also the build will run out of memory if you do not increase the size of the swap file (=increase virtual memory size)
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

Download and compile PyTorch. This will take a while ...
```
cd
git clone --recursive https://github.com/pytorch/pytorch
cd pytorch
python3.7 setup.py build
sudo -E python3 setup.py install

```
