Pre-Setup
==

## Disclaimer

To follow this tutorial you have to have installed a Linux distribution, Debian 7.9 is the one used here, any Debian based distribution should work, the instructions provided for the Linux distributions supported for Yocto project are shown (and tested successfully) but not followed on this document.

## Adjusting our Linux

This section provides required set up and packages for building an image with Real-Time kernel patch. 

### Debian and Debian based (supported) distributions

The following packages are needed by a Debian or Debian based distribution, to install them, update and install:

    sudo apt-get update
    sudo apt-get install build-essential git diffstat gawk chrpath texinfo libtool gcc-multilib dfu-util screen

### Fedora (supported) distributions

The following packages are needed by a Fedora distribution, to install them, update and install:
For newer versions of Fedora where ```yum``` is deprecated, please us ```dnf``` instead of ```yum```.

    sudo yum update
    sudo yum install gawk make wget tar bzip2 gzip python unzip perl patch    diffutils diffstat git cpp gcc gcc-c++ glibc-devel texinfo chrpath ccache perl-Data-Dumper perl-Text-ParseWords perl-Thread-Queue

### OpenSUSE (supported) distributions

The following packages are needed by an OpenSUSE distribution, to install them, update and install:

    sudo zypper update
    sudo zypper install python gcc gcc-c++ git chrpath make wget python-xml diffstat texinfo python-curses patch

### CentOS (supported) distributions

The following packages are needed by a CentOS distribution, to install them, update and install:

    sudo yum update
    sudo yum install make docbook-style-dsssl docbook-style-xsl docbook-dtds docbook-utils fop libxslt dblatex xmlto


## Download Edison Image

To download the Edison image, go to the desired working directory and download it.
In this case, we move to our home directory, a new folder is created as a Workspace, and then we move to this new Workspace directory

    $ cd ~
    $ mkdir Workspace
    $ cd Workspace

In this *Workspace* directory the Edison image is downloaded

    $ wget downloadmirror.intel.com/24389/eng/edison-src-rel1-maint-rel1-ww42-14.tgz
    $ wget http://downloadmirror.intel.com/25028/eng/edison-src-ww25.5-15.tgz
