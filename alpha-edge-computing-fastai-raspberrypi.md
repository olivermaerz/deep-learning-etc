# Edge computing with fast.ai v1 and PyTorch on a Raspberry Pi 3


## Install Python 3.7 
Details here https://gist.github.com/dschep/24aa61672a2092246eaca2824400d37f plus here https://stackoverflow.com/questions/27022373/python3-importerror-no-module-named-ctypes-when-using-value-from-module-mul

Install some required packages with apt
```
sudo apt-get install build-essential tk-dev libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libsqlite3-dev libssl-dev libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev python-dev python-setuptools python-pip python-smbus libffi-dev
sudo apt-get install libopenblas-dev cython libatlas-dev m4 libblas-dev cmake python3-yaml
```

Compile and install 
```
wget https://www.python.org/ftp/python/3.7.1/Python-3.7.1.tgz
tar -xzvf Python-3.7.1.tgz
cd Python-3.7.1/
clear
./configure --enable-optimazations
make -j 4
sudo make altinstall
```

## Install PyTorch for CPU (nightly is required by fast.ai v1)

Details here: https://gist.github.com/fgolemo/b973a3fa1aaa67ac61c480ae8440e754

First export compilation flages to disable gpu and distributed computing
```
export NO_CUDA=1
export NO_DISTRIBUTED=1
```


