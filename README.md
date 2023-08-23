# Jetson Nano Bird Camera
Real-time artificial intelligence algae classification system based on Jetson Nano

## 1. Items Needed to Build an AI Camera
<img src = "/img/JetsonNano+Camera.jpg" width="300" height="300">

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

Change the top part as shown below.
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

#Change the top part as shown below.
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
(Inside the Darknet folder)
```bash
git clone https://github.com/AlexeyAB/Yolo_mark
cd Yolo_mark

cmake .
make
chmod u+x ./linux_mark.sh 
```
```bash
cd Yolo_mark/x64/Release/data
vim obj.data

#Change the top part as shown below.
classes= 1
....
```
```bash
vim obj.names

#Change the top part as shown below.
Crow
Azure-winged magpie
Brown-eared bulbul
Magpie
Pigeon
Eurasian Jay
Tree sparrow
Chickadees
```
(Before running image labeling, copy all 400 bird images downloaded in Step 5 to the 'Yolo_mark/x64/Release/data/img' folder. Delete the image that was contained in the existing 'img' folder.)
```bash
cd ../../../
./linux_mark.sh
```
Use the mouse to display only the bird area as shown in the picture below.

<img src = "/img/label.png" width="480" height="240">

Copy the following four files from the path of 'Yolo_mark/x64/Release/data' to the path of 'darknet/data'.
1. 'img' folder
2. obj.data
3. obj.names
4. train.txt

## 7. Learning Bird Image

### 7-1. Setting up Darknet
```bash
cd darknet/cfg
vim yolov3-tiny.cfg

#Change the top part as shown below.
max_batches=16200
steps=12960, 14580
...
[convolutional]
filters=39
....
[yolo]
classes=8
....
```
```bash
cd ../
cd data
vim train.txt

#Enter the command below in Vim.
:%s/x64\/Release\///i
```
```bash
wget https://pjreddie.com/media/files/darknet53.conv.74
./darknet detector train data/obj.data cfg/yolov3-tiny.cfg darknet53.conv.74 -gpu 0 
```
The graph below appears when the learning begins. The graph in the picture is the graph after the learning is completed.

<img src = "/img/chart_yolov3-tiny.png" width="300" height="300">

### 7-2. Learning

### 7-3. Test

## 8. Copy to Jetson Nano

### 8-1. Test

### 8-2. Connect Camera

### 8-3. Real-time Test
