Installation
==

First, make sure we are at the current path and have the file we just downloaded.

    $ pwd
    /home/iotchampion/Workspace
    $ ls
    edison-src-ww25.5-15.tgz
    
Extract the contents of the edison-src-rel1-maint-rel1-ww42-14.tgz file you just downloaded and change directory to the one just extracted.
    
    $ tar xvf edison-src-ww25.5-15.tgz
    edison-src/
    edison-src/Makefile
    edison-src/meta-intel-edison/
    edison-src/meta-intel-edison/README
    edison-src/meta-intel-edison/MAINTAINERS
            .
            .
            .
    edison-src/meta-intel-edison/meta-intel-edison-bsp/recipes-kernel/linux/files/
    edison-src/meta-intel-edison/meta-intel-edison-bsp/recipes-kernel/linux/files/upstream_to_edison.patch
    edison-src/meta-intel-edison/meta-intel-edison-bsp/recipes-kernel/linux/files/defconfig
    edison-src/meta-intel-edison/meta-intel-edison-bsp/README.sources
    
    $ ls
    edison-src  edison-src-ww25.5-15.tgz
    
    $ cd edison-src/
    $ pwd
    /home/iotchampion/Workspace/edison-src
    $ ls
    Makefile  meta-intel-edison

Connect two USB cables to the Edison board and to the computer where the commands are executing, move the switch next to the microUSBs slots towards the microUSBs.

Use the setup.sh script that is inside the folder *meta-intel-edison*. This script initializes the build environment for Edison. Type

    $ ./meta-intel-edison/setup.sh
    We are building in external mode
    Cloning into bare repository 'poky-mirror.git'...
    remote: Counting objects: 297154, done.
    remote: Compressing objects: 100% (73648/73648), done.
    remote: Total 297154 (delta 218863), reused 296043 (delta 217752)
    Receiving objects: 100% (297154/297154), 116.33 MiB | 4.84 MiB/s, done.
            .
            .
            .
    Now run these two commands to setup and build the flashable image:
    cd /home/iotchampion/Workspace/edison-src/out/linux64
    source poky/oe-init-build-env
    bitbake edison-image
    *************

to run it. Optionally, we can move our download and build cache (also known as sstate) directories under the build directory. Moving these two directories will make it easier to share data between build environments and allow much faster rebuilding images

    $ mkdir bitbake_download_dir
    $ mkdir bitbake_sstate_dir
    $ ./meta-intel-edison/setup.sh --dl_dir=bitbake_download_dir --sstate_dir=bitbake_sstate_dir

Then, change directory to poky, see its content

    $ cd out/linux64/
    $ pwd
    /home/iotchampion/Workspace/edison-src/out/linux64
    $ ls
    build  poky
    
and configure the shell environment with the following source command

    $ source poky/oe-init-build-env
    ### Shell environment set up for builds. ###

    You can now run 'bitbake <target>'

    Common targets are:
        core-image-minimal
        core-image-sato
        meta-toolchain
        adt-installer
        meta-ide-support
    
    You can also run generated qemu images with a command like 'runqemu qemux86'

Now, we are ready to build a full Edison image with the following bitbake command
    
    $ bitbake edison-image
    Loading cache: 100% |###################################################################################################| ETA:  00:00:00
    Loaded 1365 entries from dependency cache.
    NOTE: Resolving any missing task queue dependencies
    
    Build Configuration:
    BB_VERSION        = "1.24.0"
    BUILD_SYS         = "x86_64-linux"
            .
            .
            .
    NOTE: Tasks Summary: Attempted 3757 tasks of which 568 didn't need to be rerun and all succeeded.

    Summary: There were 26 WARNING messages shown.
    
    
    

It is important to build a full image for the first time before making any changes to the Edison image. Be patient, this process takes from 2 to 5 or more hours depending on the hardware of the host machine.

After successfully building the edison-image, we have to modify the postBuild.sh script in order to have the correct paths. Let's change directory and verify we are editing the correct file.

    $ ls
    bitbake.lock  cache  conf  tmp
    $ cd ../../../
    $ pwd
    /home/iotchampion/Workspace/edison-src
    $ ls
    bbcache  Makefile  meta-arduino  meta-intel-edison  out

Use your favorite simple text editor and modify line 9 of the postBuild.sh script. In this case we use vim.

    $ vim meta-intel-edison/utils/flash/postBuild.sh

Once inside, type ```i``` to insert new text, modify line 9 so it looks like this.

    7 build_dir=""
    8 if [ $# -eq 0 ]; then
    9   build_dir=$top_repo_dir/out/linux64/build
    10 else

Press escape, and use the following keyboard combination ```:wq```, then press enter.

Now, run the postBuild script with the following command.

    $ meta-intel-edison/utils/flash/postBuild.sh
    EDISON_ROOTFS_MB = 1536, IMAGE_SIZE_MB = 533
    1+0 records in
            .
            .
            .
       Image 0: 14675 Bytes = 14.33 kB = 0.01 MB
    **** Done ***
    Files ready to flash in /home/iotchampion/Workspace/edison-src/out/linux64/build/toFlash/
    Run the flashall script there to start flashing.
    *************

Make sure we are in this path 
    
    $ pwd /home/iotchampion/Workspace/edison-src
    /home/iotchampion/Workspace/edison-src
    

Disconnect the two USB cables to the Edison board and the computer where the commands are executing, connect them after a few seconds and make sure the switch next to the microUSBs slots is-towards the microUSBs.
    
And finally Flash Intel Edison image

    $ ./out/linux64/build/toFlash/flashall.sh
    Using U-Boot target: edison-blankcdc
    Now waiting for dfu device 8087:0a99
    Please plug and reboot the board
            .
            .
            .
    Flashing boot partition (kernel)
    Flashing rootfs, (it can take up to 5 minutes... Please be patient)
    Rebooting
    U-boot & Kernel System Flash Success...
    Your board needs to reboot to complete the flashing procedure, please do not unplug it for 2 minutes.

Let's change to our edison-src folder and verify we see these files:
    
    $ cd ~/Workspace/edison-src/
    $ pwd
    /home/iotchampion/Workspace/edison-src
    $ ls
    bbcache  build  flash.log  Makefile  meta-arduino  meta-intel-edison  out
    
Create a directory called Patches and then switch to it

    $ mkdir Patches
    $ cd Patches
    $ pwd
    /home/iotchampion/Workspace/edison-src/Patches

and use wget to download the Real Time patches

    $ wget http://yoneken.sakura.ne.jp/share/rt_edison.tar.bz2
    --2016-01-06 13:27:36--  http://yoneken.sakura.ne.jp/share/rt_edison.tar.bz2
    Resolving yoneken.sakura.ne.jp (yoneken.sakura.ne.jp)... 219.94.129.103
    Connecting to yoneken.sakura.ne.jp (yoneken.sakura.ne.jp)|219.94.129.103|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 686080 (670K) [application/x-bzip2]
    Saving to: `rt_edison.tar.bz2'
    
    100%[=========================================================================================================================================================================>] 686,080      325K/s   in 2.1s    
    
    2016-01-06 13:27:38 (325 KB/s) - `rt_edison.tar.bz2' saved [686080/686080]
    
Decompress the bz2 file and see we have the following files:

    $ tar -xaf rt_edison.tar.bz2
    $ ls
    intel_mid_rpmsg.c.patch  patch-3.10.17-rt12_edison.patch  rt_edison.tar.bz2

and move these two patches called *patch-3.10.17-rt12_edison.patch* and *intel_mid_rpmsg.c.patch* to the files directory where new patches are placed:

    $ cp *.patch ../meta-intel-edison/meta-intel-edison-bsp/recipes-kernel/linux/files/

Verify that the destination folder has these two patches inside it.

    $ ls ../meta-intel-edison/meta-intel-edison-bsp/recipes-kernel/linux/files/
    defconfig  intel_mid_rpmsg.c.patch  patch-3.10.17-rt12_edison.patch  upstream_to_edison.patch

Now, let's edit a file under the Linux directory, one directory above the files directory we've just copied the Real Time patches. Move to the Linux directory

    $ cd ../meta-intel-edison/meta-intel-edison-bsp/recipes-kernel/linux/

and edit de bbappend file (now we use another editor called gedit  for variety purposes, assuming the edition happens under a Debian based Linux distribution; otherwise use a simple text editor like nano, vi, vim or emacs)

    $ gedit linux-yocto_3.10.bbappend
    
Replace the content of the file to have the following:

    FILESEXTRAPATHS_prepend := "${THISDIR}/files:"  
    COMPATIBLE_MACHINE = "edison"  
    LINUX_VERSION = "3.10.17"  
    SRCREV_machine = "c03195ed6e3066494e3fb4be69154a57066e845b"  
    SRCREV_meta = "6ad20f049abd52b515a8e0a4664861cfd331f684"  
      
    SRC_URI += "file://defconfig"  
    SRC_URI += "file://upstream_to_edison.patch"  
    SRC_URI += "file://patch-3.10.17-rt12_edison.patch"  
    SRC_URI += "file://intel_mid_rpmsg.c.patch"  
    
    do_configure() {  
      cp "${WORKDIR}/defconfig" "${B}/.config"  
    }  
    
    do_kernel_configme() {  
      cp "${WORKDIR}/defconfig" "${B}/.config"  
    }  
    
    do_patch() {  
      cd ${S}  
      git am "${WORKDIR}/upstream_to_edison.patch"  
      git apply "${WORKDIR}/patch-3.10.17-rt12_edison.patch"  
      git apply "${WORKDIR}/intel_mid_rpmsg.c.patch"  
    }

Save the file and exit.

Now, move to our edison-src root folder
    
    $ pwd
    /home/iotchampion/Workspace/edison-src/meta-intel-edison/meta-intel-edison-bsp/recipes-kernel/linux
    $ cd ../../../../
    $ pwd
    /home/iotchampion/Workspace/edison-src/
    $ make setup
    $ make

Move to the linux64 folder,

    $ cd out/linux64/
    $ pwd
    /home/iotchampion/Workspace/edison-src/out/linux64

configure the shell environment again

    $ source poky/oe-init-build-env
    
    ### Shell environment set up for builds. ###

    You can now run 'bitbake <target>'
    
    Common targets are:
        core-image-minimal
        core-image-sato
        meta-toolchain
        adt-installer
        meta-ide-support
    
    You can also run generated qemu images with a command like 'runqemu qemux86'

and get into the Kernel Configuratin

    $ bitbake virtual/kernel -c menuconfig

When first run, you will be prompted with a screen like this

 ![Kernel Configuration](Images/menuconfig1.png)

Enable *Control Group Support* under General setup settings

    General setup  --->
     -*- Control Group support  --->

 ![Control Group Support](Images/menuconfig2.png) 

Enable *High Resolution Timer Support* under General setup -> Timer subsystem settings

    General setup  --->
     Timers subsystem  --->
      [*] High Resolution Timer Support

 ![High Resolution Timer Support](Images/menuconfig3.png)

Enable *Fully Preemptible Kernel (RT)* under Processor type and features settings

    Processor type and features  --->
     Preemption Model (Fully Preemptible Kernel (RT))  --->
      (X) Fully Preemptible Kernel (RT)

 ![Fully Preemptible Kernel](Images/menuconfig4.png)

Enable Timer frequency to *1000 HZ* under Processor type and features -> Timer frequency settings

    Processor type and features  --->
     Timer frequency (100 HZ)  --->
      (X) 1000 HZ

 ![Fully Preemptible Kernel](Images/menuconfig5.png)

Disable *ACPI (Advanced Configuration and Power Interface)* under Power management and ACPI options settings

    Power management and ACPI options  --->
     [ ] ACPI (Advanced Configuration and Power Interface) Support  --

 ![ACPI](Images/menuconfig6.png)

Disable *APM (Advanced Power Management) BIOS support* under  settings

    Power management and ACPI options  --->
     < > APM (Advanced Power Management) BIOS support  --->

 ![Fully Preemptible Kernel](Images/menuconfig7.png)

Disable *ALSA for SoC audio support* under Device Drivers -> Sound card support -> Advanced Linux Sound Architecture -> ALSA for SoC audio support settings

    Device Drivers  --->
     <*> Sound card support  ---> 
      < >   Advanced Linux Sound Architecture  --->

 ![ALSA for SoC audio support](Images/menuconfig8.png)

Disable *Aufs (Advanced multi layered unification filesystem) support* under File systems -> Miscellaneous filesystem -> Aufs (Advanced multi layered unification filesystem) support settings

    File systems  --->
     [*] Miscellaneous filesystems  --->
      < >   Aufs (Advanced multi layered unification filesystem) support

 ![Aufs](Images/menuconfig9.png)

Select __< Save >__ to keep the Kernel Configuration and then select __< Exit >__ to go back to your console

When the Kernel configuration is complete, change directory to linux-edison-standard-build folder

    $ cd tmp/work/edison-poky-linux/linux-yocto/3.10.17+gitAUTOINC+6ad20f049a_c03195ed6e-r0/linux-edison-standard-build/

and copy the Kernel configuration to these two folders

    $ cp .config /home/iotchampion/Workspace/edison-src/meta-intel-edison/meta-intel-edison-bsp/recipes-kernel/linux/files/defconfig
    $ cp .config ../linux/arch/x86/configs/i386_edison_defconfig

Go back to our linux64 folder

    $ cd ~/Workspace/edison-src/out/linux64
    $ pwd
    /home/iotchampion/Workspace/edison-src/out/linux64

and configure the shell environment again

    $ source poky/oe-init-build-env

Force bitbake to copy the modified configuration to the actual build directory.

    $ bitbake virtual/kernel -c configure -f -v

Now our image is ready to be built.
    
    $ bitbake edison-image

The whole Edison image is rebuilt using the Real Time patched Kernel.

Now, we have to run a post building script, located in another folder. Change directory to

    $ cd ../../../meta-intel-edison/utils/

and run
    
    $ ./postBuild.sh

to prepare our new setup for the flashing process. Change directory to the flash folder

    $ cd flash/

and execute the flash script with sudo privileges

    $ sudo ./flashall.sh 

Finished the flashing process, get into de Edison system

    $ sudo screen /dev/ttyUSB0 115200

hit enter a few times and a log in appears. Default user is *root* with no password.

Once logged in run ```uname -a```, the name of the Kernel should have been renamed with the RT tags as shown below.

![uname](Images/uname.PNG)

*******************************
## Version ww42-14

Go to your home directory

    $ cd

Untar...

    $ tar -xzf edison-src-rel1-maint-rel1-ww42-14.tgz
    $ ls edison-src
    arduino  broadcom_cws  device-software  mw
    $ cd edison-src
    $ ./device-software/setup.sh
    $ gedit ./build/conf/local.conf
    # Modify 'BB_NUMBER_THREADS = "16"' and 'PARALLEL_MAKE = "-j 12"'  
    $ source poky/oe-init-build-env
    $ bitbake edison-image
    $ ../device-software/utils/flash/postBuild.sh

## Version ww24-15

    $ cd
    $ tar xvf edison-src-ww25.5-15.tgz
    $ ls edison-src
    Makefile  meta-intel-edison
    $ cd edison-src



*******************************
