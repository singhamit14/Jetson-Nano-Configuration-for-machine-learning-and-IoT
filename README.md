# Jetson_Nano_Configuration
A comprehensive guide for configuring the Jetson Nano, including all necessary steps, tools, and scripts to set up and optimize your device for various AI and robotics projects.


# 1 - If you have Jetson Nano Developer Kit
1- Download the jetson nano sd card image from this link - (https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#prepare)
2- Write this image on sdcard
3- And then boot it.

# 2 - If you have Jetson Nano Developer Kit with emmc storage
Step 1- For this you required a hot machine which have linux operating system 18.04 or 22.04.
Step 2- On host machine first install the nvidia SDK manager through this link - (https://developer.nvidia.com/sdk-manager).
Step 3- Create a account on it.
Step 4- And follow this tutorial for batter understanding- 1- (https://youtu.be/D0v1q-HUM4o?si=YucGI65XZt24ui1K)  2- (https://youtu.be/XTjh8Km7coM?si=yXckPHxrs73oL8VZ).

Step 5- After following above tutorial, open new terminal in jetson nano (And here we enable sd card option in jetson nano because in this we have install so many library for that we required more space but in emmc board they have only 16 or 32 gb storage for that we need to boot our jetson nano from sd card for that follow below step).

# 2-i Steps to enable SD card:- 
    1  git clone https://github.com/Seeed-Studio/seeed-linux-dtoverlays.git
    2  cd seeed-linux-dtoverlays
    3  sed -i '17s#JETSON_COMPATIBLE#\"nvidia,p3449-0000-b00+p3448-0002-b00\"\, \"nvidia\, jetson-nano\"\, \"nvidia\,tegra210\"#' overlays/jetsonnano/jetson-sdmmc-overlay.dts
    4  make overlays/jetsonnano/jetson-sdmmc-overlay.dtbo
    5  sudo cp overlays/jetsonnano/jetson-sdmmc-overlay.dtbo /boot/
    6  cd /boot/
    7  sudo /opt/nvidia/jetson-io/config-by-hardware.py -l
    8  sudo /opt/nvidia/jetson-io/config-by-hardware.py -n "reComputer sdmmc"
    9  sudo reboot

# 2-ii Check sdcard showing or not-
1.	sudo jetson_clocks
2.	gnome-disks

# 2-iii Steps to get SD card bootable:-(new terminal)
    1  df -h  (check that memory of 64 gb is showing)
    2  git clone https://github.com/limengdu/bootFromUSB
    3  cd bootFromUSB
    4  ./copyRootToUSB.sh -p /dev/mmcblk1p1

         Note:- wait until all the data copied to SD card

     5  sudo gedit /boot/extlinux/extlinux.conf

Note:- after running the above cmd a pop up window will appear on the screen change the emmc memory name to sd card memory name  eg in place of 0 write 1 in memory address name in both the places in above and the below and then save the file and close the pop-up screen.
Example: In place of mmcblk0p1 write mmcblk1p1.

    6  reboot



# 2-iv Upgrade pip and setuptools (In new terminal)-
1.	python3 –m pip install  --upgrade pip
2.	python3 –m pip install  --upgrade  setuptools


# Export cuda in terminal-

1.	export PATH=/usr/local/cuda-10.2/bin${PATH:+:${PATH}}
2.	export LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

# Installing opencv with cuda –
 Follow this link for installing cuda with opencv.
1.	https://automaticaddison.com/how-to-install-opencv-4-5-on-nvidia-jetson-nano/

# Installing pytorch and torchvision –
Pytorch
1.	wget https://nvidia.box.com/shared/static/p57jwntv436lfrd78inwl7iml6p13fzh.whl -O torch-1.8.0-cp36-cp36m-linux_aarch64.whl
2.	sudo apt-get install python3-pip libopenblas-base libopenmpi-dev libomp-dev
3.	pip3 install ‘Cython<3’
4.	pip3 install numpy torch-1.8.0-cp36-cp36m-linux_aarch64.whl
   
torchvision
1.	sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev libopenblas-dev libavcodec-dev libavformat-dev libswscale-dev
2.	sudo git clone --branch v0.9.0 https://github.com/pytorch/vision torchvision  
3.	cd torchvision
4.	export BUILD_VERSION=0.9.0  
5.	sudo python3 setup.py install --user
6.	cd ../  
7.	pip install 'pillow<7' 

# Installing Tensorflow –
Follow this link for installing tensorflow
1.	https://qengineering.eu/install-tensorflow-2.4.0-on-jetson-nano.html



# Installing Yolo- 
1.	https://pysource.com/2019/08/29/yolo-v3-install-and-run-yolo-on-nvidia-jetson-nano-with-gpu/

# Installing  jtop- 
1.	sudo  pip3 install –U jetson-stats 
2.	jtop


# Installing mediapipe –
Step 1- Github - https://github.com/anion0278/mediapipe-jetson
Step 3- First download the wheel file from this link --- https://github.com/anion0278/mediapipe-jetson/tree/master/dist.
And put it in home directory
Step 4- run following command in terminal
$ sudo apt update
$ sudo apt install python3-pip
$ pip3 install --upgrade pip
Step 5- Remove previous versions of Mediapipe (if it was installed):
$ pip3 uninstall mediapipe
Step 6- Install from wheel with (run commands from mediapipe dir):
$ pip3 install protobuf==3.19.4 opencv-python==4.5.3.56 dataclasses mediapipe-0.8.9_cuda102-cp36-cp36m-linux_aarch64.whl

Note: Building wheel for newer version of opencv-python may take quite some time (up to few hours)!

# Install VS Code 
1.	git clone https://github.com/JetsonHacksNano/installVSCode.git
2.	cd installVSCode
3.	./installVSCode.sh

# Install Arduino ide 
1.	git clone https://github.com/JetsonHacksNano/installArduinoIDE.git
2.	cd installArduinoIDE
3.	./installArduinoIDE.sh

# if you getting this error in your jetson nano then follow this step
‘’’’’’’’’’ if you getting --- OSError: /usr/lib/aarch64-linux-gnu/libgomp.so.1: cannot allocate memory in static TLS block
first open bashrc file --  gedit  .~/bashrc
add this line ---- export LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libgomp.so.1:$LD_PRELOAD ----- and save it 
final run the bashrc file – source .~/bashrc
adding this line to the beginning of the code ----- from torch.nn.utils.rnn import pad_sequence  ‘’’’’’’’’’’’

# If you are planing to use jetson nano with drone project so you can also follow below step (As per your reqired) 
This step for drone project –
-----https://github.com/ArthurFDLR/drone-gesture-control?tab=readme-ov-file------
Installing trt_pose –
1.	git clone https://github.com/NVIDIA-AI-IOT/trt_pose
2.	cd trt_pose
3.	sudo python3 setup.py install
Installing trt_pose –
1.	git clone https://github.com/NVIDIA-AI-IOT/torch2trt
2.	sudo pip3 install packaging
3.	cd torch2trt
4.	sudo python3 setup.py install --plugins

# Connect jetson nano with pixhwak –
1.	sudo apt-get update
2.	sudo apt-get install python-pip python3-pip
3.	sudo apt-get install python3-dev python3-opencv python3-wxgtk4.0 python3-matplotlib python3-lxml libxml2-dev libxslt-dev
4.	sudo pip install PyYAML mavproxy
5.	sudo mavproxy.py --master=/dev/ttyTHS1

 




