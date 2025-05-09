# [ubuntu-rolling](https://github.com/savoury1/ubuntu-rolling)
### Ubuntu "Rolling Release" System

To make a customized "hybrid" Ubuntu Xenial-based system that includes selected packages from newer Ubuntu series. There are many reasons to want to do this including:

* Maintain an up-to-date Ubuntu-based system without having to do full system upgrades periodically (a process that often results in many software programs no longer working, due removal of critical dependencies in the new OS version).

* Retain what is good from earlier Ubuntu-based distributions (ie. a fast and lightweight GTK2 based desktop such as in Linux Mint 18.1 Serena MATE) while also being able to use what is good and useful from newer Ubuntu series.

* Learn about the system you are using by gaining an understanding of the various software packages installed and their inter-relationships, such that you are also better equipped to fix problems as and when they might occur.

Understand that this is more a "slow roll" release rather than a "fast roll" release! So there will certainly be some who say (quite accurately, based on the traditional term) that it is not a "true rolling release" (ie. such as Arch). However, with Ubuntu not having any such form of release (ie. akin to Debian unstable/sid) there is actually something of a need for one (or even many variations of such Ubuntu "rolling release" systems, depending on what newer software packages are needed by any particular user/organization).

#### Technical background

These procedures are manual in nature, so the upgrades to packages from newer series are done manually and based on specific user selection (see the [pkg-lists](pkg-lists) directory). You as the user are effectively over-riding the default dpkg/apt setup that is (as shipped) not designed to get packages from any other Ubuntu "series" (eg. no Bionic, Cosmic, Disco or Eoan packages from a Xenial installation). For some further background technically (about the simple and easy technique used to gain access to packages from newer series) you can also see the description of the PPA here:

https://launchpad.net/~savoury1/+archive/ubuntu/ubuntu-enhanced

The source of the one package at that PPA is also in the [package](package) directory. This directory (on my own system, ie. my git clone of this project) is where I run all the debuild/pbuilder commands (the "pbuilder-all" script that is called does the work of making a "PPA acceptable" package or PAP out of the numbered version source directory used). The current package version is 0.5 and the package is now compatible with these series: Xenial, Bionic, Cosmic, Disco and Eoan.

#### [Philosophical background](PHIL101.md)

For those interested in some broader reasons as to why this Xenial-based Ubuntu "rolling release" system came about.

#### Friendly disclaimer

Please do note that this Ubuntu Xenial-based "Rolling Release" system is a "work in progress" and it is useful to state up front:

*The procedures contained on this site might break your system completely and should only be followed by people with appropriate technical backgrounds. This includes a good understanding of the Debian package management system (using "apt" commands), as well as at least a fair understanding of terminal commands and shell scripting. Any system breakage that occurs due carrying out any procedures on this site is entirely the responsibility of the person(s) carrying out those procedures.*

#### System component upgrades

This table shows newer versions that can be installed on a Xenial-based distribution, through this modular upgrade process:

System component | Xenial version | Bionic upgrade | Cosmic upgrade | Disco upgrade | Eoan upgrade
---------------- | -------------- | -------------- | -------------- | ------------- | ------------
C libraries | 2.23 | 2.27 | 2.28 | 2.29 | 2.30
systemd | 229 | 237 | 239 | 240 | 242
GTK3 | 3.18.9 | 3.22.30 | 3.24.4 | 3.24.8 | 3.24.11
GPG | 1.4.20 | 2.2.4 | 2.2.8 | 2.2.12 | 2.2.12
X.Org X Server | 1.18.4 | 1.19.6 | 1.20.1 | 1.20.4 | 1.20.5
KDE libraries (apps) | 5.18.0 (2.0.3) | 5.44.0a (17.12.3) | 5.50.0 (18.04.3) | 5.56.0 (18.12.3) | 5.62.0 (19.04.3)
Qt libraries | 5.5.1 | 5.9.5 | 5.11.1 | 5.12.2 | 5.12.4

![Serena Enhanced](images/Serena-Enhanced.png)
*This screenshot is from the test system used for creating these procedures (as at Aug 10th, 2019). As can be seen, there is a mix of packages from four Ubuntu "series" currently installed, including approximately 2/3rd Bionic, 1/10th Disco and 1/8th of the system being from the yet-to-be-released Eoan (shipping Oct 17th, 2019)!*

Many new software packages especially require newer KDE/Qt libraries than can be installed from the Xenial repositories. For instance, digiKam (excellent photo management software) 5.9.0 (a very stable version) requires Qt 5.9 making it impossible to run a native version of this program on Xenial systems. An AppImage is provided on the digiKam website that works well, however, many programs that similarly require newer Qt libraries than in Xenial do not have an AppImage available, so being able to actually upgrade the system Qt libraries to a much newer version is very advantageous.

Additionally relative to Qt, the qt5ct tool that allows consistent theming of Qt applications does not work until Qt 5.7 meaning that the visual appearanace of all Qt applications on pure Ubuntu Xenial systems can not be defined by the user. An annoying fact when using various Qt based software such as VirtualBox Manager. By upgrading Qt to the Bionic version, a Xenial-based system can then use qt5ct to define themes, font sizes and more in a consistent fashion for all Qt applications. If you like to have dark themes with light text (much better for the eyes and brain!) then Qt 5.7+ is therefore a must.

#### Overall Procedure

* Install Xenial-based system of choice (or backup existing Xenial-based install if wanting to enhance current system), though Linux Mint 18.1 Serena MATE is a recommended starting point due these procedures being created on that distribution.
* Install **ubuntu-enhanced** package from https://launchpad.net/~savoury1/+archive/ubuntu/ubuntu-enhanced PPA.
* Run `enhanced-packages-serena` (or `enhanced-packages-xenial` if using a Xenial-based distro other than Serena MATE) script to create a handful of useful custom packages that fix certain issues during/after the enhancement process.
* Run `enhancements-serena` (or `enhancements-xenial`) script to install various PPAs, upgrade various system components with Xenial versions and then call `enhancements-1st-run` (which installs about 3,000 Bionic packages with one apt command).
* Customize and modify the plain text package list files as & when needed, to choose required/desired packages from newer Ubuntu series (paying attention to package inter-dependencies).
* After customizing the package list files, run `enhance-scripts-make` to create a script `enhance-all` that can then be run periodically to keep all the selected packages from newer series up-to-date!

*See the readme file in the script directory for more detailed information and "how tos" relative running these procedures.*