## Compiling OpenCV on the Raspberry PI

# using a container to compile for the Raspberry PI
```
docker run -it -v H:/Samples/opencv4:/src/opencv arm32v7/debian
apt-get -y update
apt-get -y install software-properties-common
apt-get -y install -y wget unzip curl nano git
apt-get install -y build-essential cmake apt-transport-https

#adding the repository:
# https://www.raspbian.org/RaspbianRepository
add-apt-repository -y 'deb http://archive.raspbian.org/raspbian wheezy main contrib non-free'
wget https://archive.raspbian.org/raspbian.public.key -O - | apt-key add -
apt-get -y update

apt-get install -y zlib1g-dev libjpeg-dev libwebp-dev libpng-dev libtiff5-dev libjasper-dev libopenexr-dev libgdal-dev libdc1394-22-dev libavcodec-dev libavformat-dev libswscale-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev yasm libopencore-amrnb-dev libopencore-amrwb-dev libv4l-dev libxine2-dev libtbb-dev libeigen3-dev

```

Optional if you want the UI/windowing support (must install both in container and on the RPi)
```
apt-get -y install libgtk2.0-dev
apt-get -y install qtcreator
apt-get -y install qt5-default
```

```
# Install the prerequisites
sudo apt-get update

sudo apt-get install -y build-essential cmake apt-transport-https
sudo apt-get install -y zlib1g-dev libjpeg-dev libwebp-dev libpng-dev libtiff5-dev libjasper-dev libopenexr-dev libgdal-dev libdc1394-22-dev libavcodec-dev libavformat-dev libswscale-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev yasm libopencore-amrnb-dev libopencore-amrwb-dev libv4l-dev libxine2-dev libtbb-dev libeigen3-dev
sudo apt-get install -y wget unzip curl nano git

# Download and build OpenCV 4.0.1
cd ~
mkdir src
cd src
mkdir opencv
cd opencv
wget https://github.com/opencv/opencv/archive/4.0.1.zip
unzip 4.0.1.zip
rm 4.0.1.zip

# Download and build OpenCV_Contrib 4.0.1
wget https://github.com/opencv/opencv_contrib/archive/4.0.1.zip
unzip 4.0.1.zip
rm 4.0.1.zip

cd opencv-4.0.1/

#Build and install OpenCv
mkdir build
cd build

# no UI
cmake -DOPENCV_EXTRA_MODULES_PATH=~/src/opencv/opencv_contrib-4.0.1/modules -D WITH_LIBV4L=ON -D CMAKE_BUILD_TYPE=RELEASE -D WITH_TBB=ON -D ENABLE_NEON=ON ..
cmake -DOPENCV_EXTRA_MODULES_PATH=/src/opencv/opencv_contrib-4.0.1/modules -D WITH_LIBV4L=ON -D CMAKE_BUILD_TYPE=RELEASE -D WITH_TBB=ON -D ENABLE_NEON=ON ..

# with UI
cmake -DOPENCV_EXTRA_MODULES_PATH=/src/opencv/opencv_contrib-4.0.1/modules -D WITH_LIBV4L=ON -D CMAKE_BUILD_TYPE=RELEASE -D WITH_TBB=ON -D ENABLE_NEON=ON -DFORCE_VTK=ON -DWITH_V4L=ON -DWITH_QT=ON -DWITH_OPENGL=ON -DWITH_CUBLAS=ON -DCUDA_NVCC_FLAGS="-D_FORCE_INLINES" -DWITH_GDAL=ON -DWITH_XINE=ON -DBUILD_EXAMPLES=ON ..

# with UI + VFP optimizations:
cmake -DOPENCV_EXTRA_MODULES_PATH=/src/opencv/opencv_contrib-4.0.1/modules -D WITH_LIBV4L=ON -D CMAKE_BUILD_TYPE=RELEASE -D WITH_TBB=ON -D ENABLE_NEON=ON -DFORCE_VTK=ON -DWITH_V4L=ON -DWITH_QT=ON -DWITH_OPENGL=ON -DWITH_CUBLAS=ON -DCUDA_NVCC_FLAGS="-D_FORCE_INLINES" -DWITH_GDAL=ON -DWITH_XINE=ON -DBUILD_EXAMPLES=ON -DENABLE_VFPV3=ON ..

# LIBV4L - Enabled Video 4 Linux | TBB - Multithreading | NEON - ARM Optimizations
make
sudo make install
sudo ldconfig
```

# Save the image of the container
```
# DO NOT CLOSE THE RUNNING CONTAINER
# ensure you are logged in (docker login)

# 1. get the container id
docker ps -a

# 2. commit the running container in a new image
#    54b1a1bfe942 is the container id
docker commit 54b1a1bfe942 raf-debian-arm32v7-build

# 3. list the images and get the image id
docker images

# 4. tag the image
#    raffaeler is the username
#    v1.0 is the tag
docker tag 6f467ae6f7e2 raffaeler/raf-debian-arm32v7-build:v1.0

# 5. push the image to the docker hub
#    raffaeler is the username
docker push raffaeler/raf-debian-arm32v7-build

#The final message from the last command is:
	The push refers to repository [docker.io/raffaeler/raf-debian-arm32v7-build]
	ecc1add133ea: Pushed
	1bb8a900019f: Mounted from arm32v7/debian
	v1.0: digest: sha256:39e4d6861d248d7a5dc92267b9824ca7315c58aadd23789264676a538501dde3 size: 742

```

The resulting images are:
```
raf-debian-arm32v7-build-ui                latest              dcb7c4e9b547        2 weeks ago         1.71GB
raffaeler/raf-debian-arm32v7-build-ui      v1.0                dcb7c4e9b547        2 weeks ago         1.71GB
raf-debian-arm32v7-build                   latest              6f467ae6f7e2        2 weeks ago         997MB
raffaeler/raf-debian-arm32v7-build         v1.0                6f467ae6f7e2        2 weeks ago         997MB
```

To load one of the saved containers:
```
docker run -it -v H:/Samples/opencv4:/src/opencv raf-debian-arm32v7-build-ui
```


opencvsharp
```
git clone https://github.com/shimat/opencvsharp.git
cd opencvsharp
git fetch --all --tags --prune && git checkout master


cd src
mkdir build
cd build
cmake -D CMAKE_INSTALL_PREFIX=/usr/local ..
make -j 
make install

export LD_LIBRARY_PATH="${LD_LIBRARY_PATH}:/usr/local/lib/"


# raf 
export LD_LIBRARY_PATH="${LD_LIBRARY_PATH}:/home/shimat/opencvsharp/src/build/OpenCvSharpExtern"

```

```
// a different way to get the sources:
// git clone -b '4.0.1' --single-branch --depth 1 https://github.com/opencv/opencv.git
```



```
after building the file on the container, copy all the files on the RPi
in the build folder, the .so files that are about 1-2K are just hardlinks and not actual dlls
All the hardlinks must be rebuilt on the RPi. Therefore delete those files on the copied RPi
and then re-run locally `make`
Then you run `make install` (which will register the libraries in the /usr/local folder)
And finally `ldconfig`.
```







Privileged mode in docker / raspberry:
https://iotbytes.wordpress.com/create-your-first-docker-container-for-raspberry-pi-to-blink-an-led/


to enable usb driver of the RPi camera:
```
sudo modprobe bcm2835-v4l2
```

## FFMpeg with h264

http://jollejolles.com/installing-ffmpeg-with-h264-support-on-raspberry-pi/

### Install h264 library

Open a terminal window on the raspberrypi (or via SSH connection) and type in the following commands:

1. Download h264 library: `git clone --depth 1 http://git.videolan.org/git/x264`
2. Change directory to the x264 folder: `cd x264`
3. (NO) Configure installation: `./configure --host=arm-unknown-linux-gnueabi --enable-static --disable-opencl`
3. Configure installation: `./configure --host=arm-unknown-linux-gnueabi --enable-shared`
4. Create the installation: `make -j4`
5. Install h264 library on your system: `sudo make install`

```
install -d /usr/local/bin
install x264 /usr/local/bin
install -d /usr/local/include /usr/local/lib/pkgconfig
install -m 644 ./x264.h x264_config.h /usr/local/include
install -m 644 x264.pc /usr/local/lib/pkgconfig
install -d /usr/local/lib
install -m 644 libx264.a /usr/local/lib
gcc-ranlib /usr/local/lib/libx264.a
```

### Install ffmpeg with h264

1. Change to home directory: `cd ~`
2. Download ffmpeg: `git clone git://source.ffmpeg.org/ffmpeg --depth=1`
3. Change to ffmpeg directory: `cd ffmpeg`
4. Configure installation: `./configure --arch=armel --target-os=linux --enable-gpl --enable-libx264 --enable-nonfree --enable-shared`
5. Make the installation: `make -j4`. Note this step will take a long time!
6. Now finally run the installation: `sudo make install`

The option `--enable-shared` is mandatory to obtain the shared libraries `.so` as opposed to the static libraries `.a`.

The project `FFmpeg.AutoGen` needs the dynamic libraries.



### Test ffmpeg
```
ffmpeg -i raw.h264 -vcodec copy output.mp4
```


### On every reboot (or make it automatic)

```
sudo su
sudo modprobe bcm2835-v4l2
export LD_LIBRARY_PATH="${LD_LIBRARY_PATH}:/usr/local/lib/"
export ASPNETCORE_ENVIRONMENT=Development

```


### General notes

```
For gcc 4.8 the most optimized version of switches are:
gcc-4.8 -mcpu=cortex-a7 -mfpu=neon-vfpv4 -mfloat-abi=hard
`` 