# JetsonNano_Bird_Camera
Real-time artificial intelligence algae classification system based on Jetson Nano

## 1. Items Needed to Build an AI Camera
1. Ubuntu PC with Nvidia GPU
2. Jetson Nano (Recommend 4GB Model)
3. USB Camera or Raspberry Pi Camera v2
   
   etc...

## 2. Install CUDA and CUDNN
Install CUDA and CUDNN for learning using GPU.

### 2-1. Ubuntu PC

### 2-2. Jetson Nano

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

### 4-2. Jetson Nano

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
