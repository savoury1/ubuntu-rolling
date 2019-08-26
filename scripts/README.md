## Base Enhancement

To enhance your own Xenial-based distribution with packages from newer Ubuntu series:

* Copy all scripts from "base-enhancement" directory and all patches from "base-enhance-patches" directory into one location, eg. "~/Downloads" is a good choice.
* See https://launchpad.net/~savoury1/+archive/ubuntu/xenial-enhanced for installation of the xenial-enhanced package that installs text configuration files into /etc/apt/ (to make packages from the newer Ubuntu series available, ie. Bionic/Cosmic/Disco/Eoan) as well as an **`apt-new`** command allowing easy installation of packages from newer series. Proceed to the next step after installing the xenial-enhanced package.
* Run **`enhanced-packages-serena`** (or **`enhanced-packages-xenial`** if using a Xenial distribution other than Mint Serena MATE as the starting point) to create several custom packages used by the enhancement script in the next step.
* Run **`enhancements-serena`** (or **`enhancements-xenial`**) which installs the required PPAs, upgrades and installs required packages, then calls **`enhancements-1st-run`** to do the main work of upgrading almost 3,000 packages to Bionic versions.
* The **`enhancements-1st-run`** script will take some time! In testing, it took about 90 minutes in a virtual machine on an i7 processor with fast SSD. This is the one long part of these procedures and once done it is quick and easy to upgrade sub-components of the OS.
* Once **`enhancements-1st-run`** completes it will drop back to the **`enhancements-serena`** (or **`enhancements-xenial`**) script for some final actions, including re-install of packages removed prior to the upgrade process (due version conflicts).
* Reboot to your new "enhanced" Ubuntu "rolling release" distribution!

&nbsp;

## Enhance Scripts Make

To create your own custom enhancement scripts based on your choices of packages from newer Ubuntu series:

* Copy contents of "enhance-scripts-make" directory to a location in the path for user scripts (eg. "~/.local/bin").

* Make a directory "~/.enhance" and copy "../pkg-lists" directory into that directory (this location can be changed to another if you prefer, which will require editing the "package_lists_dir" variable in **`enhance-scripts-make`**).

* Assuming the default location, the packages listed in "~/.enhance/pkg-lists/current" will be used to create the enhancement scripts **`enhance-all`** (one apt command per series, faster to run) and **`enhance-cat`** (one apt command per category per series, much slower to run though good for debugging purposes).

* Run **`enhance-scripts-make`** to actually create the **`enhance-all`** and **`enhance-cat`** scripts, which will (by default) be created in "~/.local/bin" (again, this location can be changed, by editing the "enhance_script_dir" variable in **`enhance-scripts-make`**).

&nbsp;

## Editing package lists

The format of the package list files is very simple. A space delimited text file with the first field being the single first letter (in uppercase specifically!) of the required series for the package (or one of two special characters being "-" to signify do not install and "~" to signify that a modded version of this package is needed) and the second field being the exact Ubuntu package name. For example, in the "applications.txt" file:
```
D meld -- latest available version
B 4pane
```
This says to install meld from Disco and 4pane from Bionic. Note that any additional text after the package name becomes a third comments field (in this case with meld "-- latest available version") and is ignored by **`enhance-scripts-make`** though useful for putting notes in about inter-dependencies or other reasons that some package(s) comes from a specific series.

Looking into the package list files will quickly show how this works. There is currently minimal sanity checking of the package lists by **`enhance-scripts-make`** (a basic check that the first field is either a valid series or the dash/tilde symbol) so take care with editing the text in these files.

&nbsp;

## Package lists directories

The "pkg-lists/bionic" directory contains a baseline Bionic configuration, as is installed by **`enhancements-1st-run`** in the initial enhancement process. The "pkg-lists/current" directory contains a duplicate of that base configuration (thus the Bionic copy is effectively the baseline backup). Then "pkg-lists/tweaked" contains the latest live package lists being used on the system used to create these enhancement or "rolling release" procedures (see the next section for how to switch to the "tweaked" package lists).

To create and use a new set of package lists is simple. In the "pkg-lists" directory copy one of the existing directories (ie. the "bionic" directory, starting from baseline; or the "current" directory, replicating your own current package selections; or the "tweaked" directory, replicating the latest package selections on the test system) to a new directory. Then, either rename the "current" directory to something else and the new directory to "current" OR edit the "package_lists_dir" variable in **`enhance-scripts-make`** to point to the new directory name.

&nbsp;

## Extra enhancement

For those wanting newest Ubuntu repo KDE/Qt as well as a selection of other newer useful packages (eg. critical security libraries) there is an enhancements extra script that adds additional PPAs and also upgrades many libraries to Disco/Eoan versions.

* Copy contents of "extra-enhancement" directory into one location, eg. "~/Downloads" is a good choice.

* Run **`enhancements-extra-ppa`** to add several additional useful PPAs that allow further upgrade of several more components of the system. This script will also modify two PPA Python 3 packages to install successfully.

* Then the following commands can be used to switch package lists to newer packages (this assumes that the package lists were copied to the recommended folder path), make the new enhance-all script and run that script to get newer packages:

```
mv "~/.enhance/pkg-lists/current" "~/.enhance/pkg-lists/regular"
ln -s "~/.enhance/pkg-lists/tweaked" "~/.enhance/pkg-lists/current"
enhance-scripts-make
enhance-all

```
* Reboot to a Xenial-based system with cutting-edge KDE/Qt allowing the running of many latest release applications!

