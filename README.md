# JetsonNano_Bird_Camera
Real-time artificial intelligence algae classification system based on Jetson Nano

## 1. Items Needed to Build an AI Camera
![image](https://onedrive.live.com/embed?resid=CF077F8837ACCF%21111718&authkey=%21AJN17fh-seZ3H-I&width=1345&height=1793)

1. Ubuntu PC with Nvidia GPU
2. Jetson Nano (Recommend 4GB Model)
3. USB Camera or Raspberry Pi Camera v2
   
   etc...

## 2. Install CUDA and cuDNN
Install CUDA and cuDNN for learning using GPU.

### 2-1. Ubuntu PC
```bash
sudo apt-get update
sudo apt install -y ubuntu-drivers-common
```
```bash
lspci | grep -i nvidia
uname -m && cat /etc/*release
gcc --version

sudo apt update
sudo apt install build-essential

uname -r
sudo apt-get install linux-headers-$(uname -r)

sudo lshw -numeric -C display
sudo ubuntu-drivers devices

sudo apt install nvidia-driver-525 #or Another version
sudo reboot

nvidia-smi #Check
```
Install Cuda Toolkit 
* Link: https://developer.nvidia.com/cuda-toolkit-archive

```bash
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
sudo sh cuda_11.8.0_520.61.05_linux.run

sudo vim ~/.bashrc
```
Add two lines at the bottom.
```bash
export PATH="/usr/local/cuda/bin:$PATH"
export LD_LIBRARY_PATH="/usr/local/cuda/lib64:$LD_LIBRARY_PATH"
```
```bash
source ~/.bashrc
nvcc -V #Check
```
Install cuDNN
* Link: https://developer.nvidia.com/rdp/cudnn-archive
* Download CUDA 11.x 'Local Installer for Linux x86_64 (Tar)'
```bash
tar -xvf cudnn-linux-x86_64-8.8.1.3_cuda11-archive.tar.xz

sudo cp cudnn-linux-x86_64-8.8.1.3_cuda11-archive/include/cudnn*.h /usr/local/cuda/include
sudo cp cudnn-linux-x86_64-8.8.1.3_cuda11-archive/lib/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*

cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2 #Check
```

### 2-2. Jetson Nano
If you are using Jetpack, no additional installation is required.
```bash
sudo vim ~/.bashrc
```
Add two lines at the bottom.
```bash
export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}$ 
export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

## 3. Install OpenCV

### 3-1. Ubuntu PC
```bash
sudo apt install build-essential cmake git pkg-config libgtk-3-dev \
    libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
    libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev \
    gfortran openexr libatlas-base-dev python3-dev python3-numpy \
    libtbb2 libtbb-dev libdc1394-22-dev libopenexr-dev \
    libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev

mkdir ~/opencv_build && cd ~/opencv_build
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
cd ~/opencv_build/opencv
mkdir -p build && cd build

cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_GENERATE_PKGCONFIG=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_build/opencv_contrib/modules \
    -D BUILD_EXAMPLES=ON ..

#make -j(Core Number)
make -j8
```

### 3-2. Jetson Nano
It takes a very long time.

```bash
wget https://github.com/Qengineering/Install-OpenCV-Jetson-Nano/raw/main/OpenCV-4-5-4.sh
sudo chmod 755 ./OpenCV-4-5-4.sh
./OpenCV-4-5-4.sh
```


## 4. Clone Darknet

### 4-1. Ubuntu PC
```bash
git clone https://github.com/AlexeyAB/darknet.git

cd darknet
sudo vim Makefile
```
Change the top part as shown below.
```bash
GPU=1
CUDNN=1
CUDNN_HALF=0
OPENCV=1
```
```bash
make
```

### 4-2. Jetson Nano
```bash
git clone https://github.com/AlexeyAB/darknet.git

cd darknet
sudo vim Makefile
```
Change the top part as shown below.
```bash
GPU=1
CUDNN=1
CUDNN_HALF=0
OPENCV=1
```
```bash
make
```

## 5. Download Bird Image
Download the image to the desired directory. (Copy to the Image Labeling folder later)

This file contains a total of 400 images of eight species, 50 for each bird species.

* Download Link: https://1drv.ms/f/s!As-sN4h_B88AhuVLb_R7KtjFNvIFNg?e=p7IoaZ

## 6. Image Labeling

## 7. Learning Bird Image


### 7-1. Setting up Darknet

### 7-2. Learning

### 7-3. Test

## 8. Copy to Jetson Nano

### 8-1. Test

### 8-2. Connect Camera

### 8-3. Real-time Test
