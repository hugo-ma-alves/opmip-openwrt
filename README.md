OPMIP package and configuration files for OpenWrt
==============================

OPMIP is an implementation of the PMIPv6 standard:

* opmip-mag - implements the Mobile Access Gateway (MAG)
* opmip-lma - implements the Local Mobility Anchor (LMA)

It is recommended to install also the odtone-openwrt

Overview
--------

opmip-OpenWrt enables Proxy mobile  capabilities in a commercial wireless
router/Access Point.

Supported Devices
-----------------

Not avaiable A yet

  
Update your Device
------------------

To install opmip capabilities in your router you need to:

* Get an appropriate image for it.
* Load this image to your device.
* Verify that everything works. 

## Getting the image


#### Building from Sources

If you prefer to build your own image rather than using one of those given,
follow the instructions below. It is strongly recommended that you build
and load a vanilla OpenWrt tree before adding any ODTONE-related functionality.

This release is based on the Attitude Adjustment OpenWrt release.

##### Building OpenWrt

Install packages required by the OpenWrt buildsystem

    apt-get install build-essential binutils flex bison autoconf gettext texinfo sharutils \
                subversion libncurses5-dev ncurses-term zlib1g-dev gawk

Checkout and prepare Attitude Adjustment and the packages. For the rest of
this section we assume that `~/projects` is your working directory. 

    cd ~/projects
    svn co svn://svn.openwrt.org/openwrt/tags/attitude_adjustment_12.09/
    cd attitude_adjustment_12.09
    ./scripts/feeds update -a
    ./scripts/feeds install -a

Configure Attitude Adjustment (select target system) and the packages

    make menuconfig

If you select extra packages you should check if you have all prerequisites
installed. Check with:

    make prereq

Finally build Attitude Adjustment

    make V=s | tee make.log

Load the image to your device. All images can be found under
`~/projects/attitude_adjustment_12.09/bin/` After the device has rebooted make
sure that you can login. Typically, this can be done as following:

    Connect to one of the "LAN" ports, not the Internet port (if there is any).
    Give your PC IP address 192.168.1.10, netmask 255.255.255.0
    Try to ping the device at 192.168.1.1
    Login to the device using "telnet 192.168.1.1" 

You should see an OpenWrt welcome message.
Make sure that your pc have an ip in 192.168.1.x range 
immediately after logon change the root password:
      
      passwd

After this logout and connect through ssh

      ssh root@192.168.1.1

Check if there is everything ok and exit.

##### Add OPMIP extensions

Configure the PATH to use the toolchain gcc instead off the host gcc:

      http://wiki.openwrt.org/doc/devel/crosscompile
      
Go to your working directory and download the ODTONE extension.

    cd ~/projects/
    git clone https://github.com/ATNoG/odtone-openwrt.git
    (I cant compile this repositorie, instead i used this:https://github.com/hugo-ma-alves/odtone-openwrt.git

Add the ompip extensions to the `attitude_adjustment_12.09` directory.

    cd ~/projects/attitude_adjustment_12.09/package/
    ln -s ~/projects/opmip-openwrt/opmip/

Add the related package to your configuration

    cd ~/projects/attitude_adjustment_12.09
    make menuconfig

Choose the following:

    Select your platform for Target System (Broadcom BRCM47xx/953XX,Atheros AR71xx, etc)
    Select your platform at Target Profile (i.e. TP-Link-WR1043ND, Broadcom BRCM43xx Wifi, etc)
    Select opmip package under network
    Save and Exit 

Build the package

    make V=s package/opmip/compile

If no errors are returned continue to the next step.

Build the image

    make V=s | tee build.log

Load the new image to your device.


## Change kernel settings to activate some extra modules
      SOON


## Loading the Image

There are different ways of loading the binary image to your device.
Please consult the related OpenWrt page and/or the OpenWrt info page
for your specific device for appropriate instructions.

#### Verifying update

By default, pre-compiled images and images build from the source code
will have the port labeled "internet" as management port (out-of-band),
with the static IP 192.168.1.1. You should be able to login through
that port as long as you configure your PC to the 192.168.1.0/24 subnet.
After you have configured your PC, try to login:

    telnet 192.168.1.1

By that time, you should be connected to the OpenWrt box which runs
opmip. Verify that the relevant processes are running:

    ps aux | grep opmip-mihf
    ps aux | grep opmip-mag



Configuration
-------------

opmip configuration files are located at `/etc/opmip folder`. It
contains the node database configuration file (node.db)



### opmip-lma

Implements the Local Mobility Anchor (LMA)

To get usage help, type at the command prompt: opmip-lma --help

      TODO

### opmip-mag

Implements the Mobile Access Gateway (MAG)

To get usage help, type at the command prompt: opmip-lma --help

      TODO

