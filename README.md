1. What's the use of do while(0) when we define a macro?
 
http://stackoverflow.com/questions/154136/do-while-and-if-else-statements-in-c-c-macros

2. How to build & install linux kernel?
 
mkdir 3.16.0-30-generic
cd 3.16.0-30-generic
sudo apt-get install dpkg-dev
apt-get source linux-image-$(uname -r) 
sudo apt-get install build-essential
sudo apt-get install kernel-package
sudo apt-get build-dep linux-image-$(uname -r)  
make oldconfig
sudo apt-get install libncurses5 libncurses5-dev 
make menuconfig  
make-kpkg
make
also refer to : http://blog.csdn.net/xin_yu_xin/article/details/42184899
