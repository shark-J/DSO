# DSO
DSO_install&amp;run
*********************************************
part 1:install
*********************************************
step 1:install opencv 3.2.0

	sudo apt-get install libopencv-dev python-opencv
  	sudo apt-get install build-essential
  	sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
  	sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
  download opencv-3.2.0 from https://opencv.org/releases/
  or you can copy the file in this github.
	
	tar xvf  opencv-3.2.0.tar.gz
  	cd opencv-3.2.0
  	cmake ..
  	make 
  	sudo make install
  !!!during install, ippicv_linux_20151201.tgz should be downloaded, place it in /home/js/opencv-3.2.0/buid/3rdparty/ippicv/ippicv_lnx
	
	sudo gedit /etc/bash.bashrc
  add PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfigexportPKG_CONFIG_PATH
  	
	source /etc/bash.bashrc
  
 **********************************************
step 2:install eigen3

	sudo apt-get install libsuitesparse-dev libeigen3-dev libboost-all-dev
  
***********************************************
step 3:install opencv-lib (install or not)

	 sudo apt-get install libopencv-dev
  
***********************************************
step 4:install Pangolin
 
 	git clone https://github.com/stevenlovegrove/Pangolin.git
	sudo apt install libgl1-mesa-dev
	cd Pangolin
	mkdir build
	cd build
	cmake ..
	cmake --build .
  
***********************************************
step 5:install DSO

	git clone https://github.com/JakobEngel/dso.git
	cd dso
	sudo apt-get install zlib1g-dev
	cd /home/js/dso/thirdparty
	tar -zxvf libzip-1.1.1.tar.gz
	cd libzip-1.1.1/
	./configure
	make
	sudo make install
	sudo cp lib/zipconf.h /usr/local/include/zipconf.h
	cd ../..	
	mkdir build
	cd build
	cmake ..
	make
**************************************************
part 2:run DSO

step 5:run with dataset

	download data from https://vision.in.tum.de/data/datasets/mono-dataset?redirect=1
	cd /home/js/dso/build/bin
	./dso_dataset \files=XXXXX/sequence_XX/images.zip \ calib=XXXXX/sequence_XX/camera.txt \ gamma=XXXXX/sequence_XX/pcalib.txt \ vignette=XXXXX/sequence_XX/vignette.png \ preset=0 \ mode=0
  
**************************************************
step 6:install 

	mkdir -p ~/catkin_ws/src
	cd ~/catkin_ws/src
	git clone https://github.com/bosch-ros-pkg/usb_cam.git 
	cd ..
	catkin_make
	source ~/catkin_ws/devel/setup.bash

**************************************************
step 7:install dso_ros

	cd ~/catkin_ws/src
	git clone https://github.com/JakobEngel/dso_ros.git
	export DSO_PATH=/home/js/dso/
	cd ..
	catkin_make
  
**************************************************
setp 8:run dso with USB cam

	t1:roscore
	t2:source ~/catkin_ws/devel/setup.bash
	roslaunch usb_cam usb_cam-test.launch
	t3:source ~/catkin_ws/devel/setup.bash
	rosrun dso_ros dso_live image:=/usb_cam/image_raw calib=/home/js/dso/camera.txt mode=1

***************************************************
PS. camera calibration using ROS
	
	t1:roscore
	t2:source ~/catkin_ws/devel/setup.bash
	roslaunch usb_cam usb_cam-test.launch
	rosrun camera_calibration cameracalibrator.py --size 8x6 --square 0.0245 image:=/usb_cam/image_raw camera:=/usb_cam
	/home/js/.ros/camera_info/head_camera.yaml
