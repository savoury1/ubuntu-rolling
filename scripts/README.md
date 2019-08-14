## Base Enhancement

To enhance your own Xenial-based distribution with packages from newer Ubuntu series:

* Copy all scripts from "base-enhancement" directory and all patches from "base-enhance-patches" directory into one location, eg. "~/Downloads" is a good choice.

* Copy "xenial-enhanced_0.2_all.deb" package from "base-enhance-package" to same directory -- this package simply installs text lists into /etc/apt/ path to make packages from the various Ubuntu series available (v0.2 adds Bionic/Cosmic/Disco/Eoan) as well as an **`apt-new`** command allowing easy installation of packages from newer series.

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

The "pkg-lists/current" directory contains the current configuration from the test system used to create the enhancement or "rolling release" procedures. The "pkg-lists/bionic" directory contains a baseline Bionic configuration, as is installed by **`enhancements-1st-run`** in the initial enhancement process.

To create a new set of package lists is simple. In the "pkg-lists" directory copy either the "bionic" directory (starting from baseline) or the "current" directory (replicating original test system) to a new directory. Then, either rename the "current" directory to something else and the new directory to "current" OR edit the "package_lists_dir" variable in **`enhance-scripts-make`** to the new directory name.

