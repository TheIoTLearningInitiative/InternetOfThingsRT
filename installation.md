# Installation

First, extract the contents of the edison-src-rel1-maint-rel1-ww42-14.tgz file you just downloaded ```tar -xzf edison-src-rel1-maint-rel1-ww42-14.tgz``` and change directory to the one just extracted ```cd edison-src``` .

Use the setup.sh script that is inside the folder *meta-intel-edison*. This script initializes the build environment for Edison. Type ```./meta-intel-edison/setup.sh``` to run it. Optionally, we can move our download and build cache (also known as sstate) directories under the build directory. Moving these two directories will make it easier to share data between build environments and allow much faster rebuilding images ```./meta-intel-edison/setup.sh --dl_dir=/path/bitbake_download_dir –-
sstate_dir=/path/bitbake_sstate_dir```  .


Then, change directory to poky ```cd out/linux64/``` and configure the shell environment with the following source command ```source poky/oe-init-build-env```. Now, we are ready to build a full Edison image with the following bitbake command  ```bitbake edison-image```   , it is important to build a full image for the first time before making any changes to the Edison image. Be patient, this process takes from 2 to 5 or more hours depending on the hardware of the host machine.


Now, let's return to our set up root folder ```cd ~/edison-src/```  . After successfully building the edison-image, run the postBuild script with the following command ```./meta-intel-edison/utils/flash/postBuild.sh``` . At this point, we are ready to flash our Edison, but we want it to have a Real Time Kernel, so we create a new directory called Patches in our current *edison-src* path ```mkdir Patches```, switch to it ```cd Patches``` and use wget command to download these patches ```http://yoneken.sakura.ne.jp/share/rt_edison.tar.bz2``` 



./meta-intel-edison/setup.sh 
ls
source poky/oe-init-build-env  
find `pwd` -type d -name 'poky'
cd out/linux64/poky/
cd ../
source poky/oe-init-build-env   

