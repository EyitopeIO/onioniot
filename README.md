# Onion Omega2 Firmware Build System

This buildsystem for the LEDE Linux distribution has been modified by Onion Corporation to build firmware for the Onion Omega2 family of devices.

![Omega2](https://github.com/OnionIoT/Onion-Media/raw/master/Product%20Photos/Omega2/OM2-2.jpg)

**Onion Corporation is not responsible for any damage to your device caused by using custom firmware or packages not built and released by Onion Corporation.**

## Notes

* This branch, `lede-17.01`, was used to build **Omega2 firmware version 0.2.2 and older**. For firmare version 0.3.0 and up, see the `openwrt-18.06` branch
* ~~Due to incompatibilities with recent kernel updates, building a firmware with the `Ralink APSoC WiFi SoftAP driver` **will cause a kernel panic during boot**~~
	* ~~This can be fixed by reflashing the Omega's firmware using the Omega's Bootloader and Ethernet Expansion~~
	* ~~We are working on a new implementation of the WiFi driver, stay tuned!~~
	* Fixed with release of Onion WiFi Warp Core driver in March 2018
		* Available as an [OpenWRT/LEDE package](https://github.com/OnionIoT/OpenWRT-Packages/tree/master/wifi-warp-core) for systems running Linux Kernel 4.4.76
		* **See [this post for more details on the Warp Core](https://onion.io/2bt-brand-new-os-release/)**

## Additional Reading

* See `CHANGELOG.md` for a listing of the changes for each firmware version and build
* See `DISCLAIMER.md` for Onion's disclaimer regarding this build system

# Using this Build System

This buildsystem can be used to create packages and firmware for the Omega2 device. There are two preferred ways to use the buildsystem:

1. Using the `onion/omega2-source` Docker image **Recommended method**
2. Running the build system on a Linux system

## Using the Docker Image

The Docker image takes care of all environmental configuration and is the recommended method. We recommend getting familiar with how Docker before trying this procedure.

Procedure:

1. Install Docker on your system: https://www.docker.com/get-started
2. Pull our Docker image by running:
```
docker pull onion/omega2-source:lede
```
3. Once the image has been pulled to your computer, you can run your own container based on the image: 
```
docker run -it onion/omega2-source /bin/bash
```
4. Your container will now be up and running, and you can compile the build system by running:
```
make
```

> **We recommend running Docker on a Ubuntu Linux system**. Some users have reported compilation issues when running Docker on Windows and Mac OS.

For more details, see: https://onion.io/2bt-cross-compiling-c-programs-part-1/

## Using a Linux System

**Not recommended for beginners** 

Procedure to setup the build system on a Ubuntu Linux System:

1. Setup Linux environment by installing the required packages:
```
sudo apt-get update
sudo apt-get install -y build-essential vim git wget curl subversion build-essential libncurses5-dev zlib1g-dev gawk flex quilt git-core unzip libssl-dev python-dev python-pip libxml-parser-perl default-jdk
```

2. Download the Build System from Github and **switch to the lede-17.01 branch**:
```
git clone https://github.com/OnionIoT/source.git
cd source
git checkout lede-17.01
```

3. Prepare build system:
```
sh scripts/onion-feed-setup.sh
git checkout .config
```
> This will initialize & configure all the package feeds as well as setup the `.config` file to match this repo. With these commands, the firmware built will match the official firmware released by Onion.

4. Compile Build System:
```
make -j
```

## Updating the Build System
*Applies to both Docker and Linux System installations*

Once you've setup your build system and have been using it, you'll want to update it from time to time.

### Updating the Package Makefiles

To grab the latest package makefiles for the [Onion Package Repo](https://github.com/OnionIoT/openwrt-packages), run this command:

```
./scripts/feeds update onion
```

> Run this command if you're seeing compilation issues errors in Onion packages, this will likely fix the problem. If not, let us know on the [Onion Community](http://community.onion.io/).

### Grabbing the Latest Build System Code

To grab the latest code from this repo, run:

```
git pull
```

If you've made modifications to which packages are built/included in the firmware, these changes will be reflected in your local `.config` file. 

**Create a backup copy of your `.config` file before proceeding.** The `git pull` command will not work unless the `.config` file is restored to it's original state by running `git checkout .config` and running `git pull` again. **Note that this will REMOVE all of your customizations!** 


## Troubleshooting

If you're encountering errors during compilation, you'll likely see something like the following:

```
make -r world: build failed. Please re-run make with -j1 V=s to see what's going on
/root/source/include/toplevel.mk:198: recipe for target 'world' failed
make: *** [world] Error 1
```

To get more visibility into the error, re-run your compilation as the error prompt suggests:

```
make -j1 V=s
```

The error messages will point you in the direction of the package responsible for the compilation error.
* If the problematic package is from Onion, see the [Updating the Package Makefiles](#updating-the-build-system) section above. This will likely resolve your issue.
* If the problematic package is not related to Onion packages, it's likely due to environment issues or issues with the code. Please check with the original authors to get pointers for debugging.

> For Docker users, note that **we recommend running Docker on a Linux System**. Some users have reported compilation issues when running the build system in Docker on Windows and Mac OS, that are not observed on Docker on Linux.

# LEDE Linux Distribution

This is the buildsystem for the LEDE Linux distribution.

Please use "make menuconfig" to choose your preferred
configuration for the toolchain and firmware.

You need to have installed gcc, binutils, bzip2, flex, python, perl, make,
find, grep, diff, unzip, gawk, getopt, subversion, libz-dev and libc headers.

Run "./scripts/feeds update -a" to get all the latest package definitions
defined in feeds.conf / feeds.conf.default respectively
and "./scripts/feeds install -a" to install symlinks of all of them into
package/feeds/.

Use "make menuconfig" to configure your image.

Simply running "make" will build your firmware.
It will download all sources, build the cross-compile toolchain, 
the kernel and all choosen applications.

To build your own firmware you need to have access to a Linux, BSD or MacOSX system
(case-sensitive filesystem required). Cygwin will not be supported because of
the lack of case sensitiveness in the file system.


Sunshine!
	Your LEDE Community
	http://www.lede-project.org


