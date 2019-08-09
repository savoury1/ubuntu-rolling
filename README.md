# [ubuntu-rolling](https://github.com/savoury1/ubuntu-rolling)
### Ubuntu "Rolling Release" System

To make a customized "hybrid" Ubuntu system with packages from newer Ubuntu series than the currently installed system. Note that this is a "work in progress" and it is useful to state an ordinary disclaimer up front:

*The procedures contained on this site might break your system completely and should only be followed by people with appropriate technical backgrounds. This includes a good understanding of the Debian package management system (using "apt" commands), as well as at least a fair understanding of terminal commands and shell scripting. Any system breakage that occurs due carrying out any procedures on this site is entirely the responsibility of the person(s) carrying out those procedures.*

With that out of the way, the purpose of the procedures on this site is to allow progressive and selective upgrading of a Ubuntu based operating system in a modular fashion. For example, starting with Linux Mint Serena 18.1 MATE (a Xenial-based distribution) released in early 2017, one can upgrade many core components of the system with Ubuntu Bionic (and later) versions. This version table gives an idea of the newer versions that are possible through this modular upgrade process:

System component | Xenial version | Bionic upgrade | Cosmic upgrade
---------------- | -------------- | -------------- | --------------
C libraries | 2.23 | 2.27 | 2.28
systemd | 229 | 237 | 239
GTK3 | 3.18.9 | 3.22.30 | 3.24.4
KDE libraries (apps) | 5.18.0 (2.0.3) | 5.44.0a (17.12.3) | 5.50.0 (18.04.3)
Qt libraries | 5.5.1 | 5.9.5 | 5.11.1

Many new software packages especially require newer KDE/Qt libraries than can be installed from the Xenial repositories. For instance, digiKam (excellent photo management software) 5.9.0 (a very stable version) requires Qt 5.9 making it impossible to run a native version of this program on Xenial systems. An AppImage is provided on the digiKam website that works well, however, many programs that similarly require newer Qt libraries than in Xenial do not have an AppImage available, so being able to actually upgrade the system Qt libraries to a much newer version is very advantageous.

Additionally relative to Qt, the qt5ct tool that allows consistent theming of Qt applications does not work until Qt 5.7 meaning that the visual appearanace of all Qt applications on pure Ubuntu Xenial systems can not be defined by the user. An annoying fact when using various Qt based software such as VirtualBox Manager. By upgrading Qt to the Bionic version, a Xenial-based system can then use qt5ct to define themes, font sizes and more in a consistent fashion for all Qt applications. If you like to have dark themes with light text (much better for the eyes and brain!) then Qt 5.7+ is therefore a must.

#### Overall Procedure

* Install Xenial-based system of choice (or backup existing install if wanting to enhance current system!)

* Update Xenial packages to latest versions (eg. ```sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade```)

* Run ```enhanced-packages-serena``` (or ```enhanced-packages-xenial``` if using straight Xenial) script to create a handful of useful custom packages that fix certain issues after enhancement

* Run ```enhancements-serena``` (or ```enhancements-xenial```) script to install various PPAs, upgrade various system components with Xenial versions and then do first "enhancement" run

* Run ```enhance-bionic``` (and additionally ```enhance-cosmic``` if desired) periodically (eg. put these scripts in ~/.local/bin) to install any updated packages (with any security patches & bugfixes) from the newer "series"

