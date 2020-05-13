See https://github.com/savoury1/ubuntu-rolling for detailed procedures relating to this package and how to (even significantly) upgrade your own Xenial-based system with packages from newer than Xenial series of Ubuntu.

-----START EXPERIMENTAL WARNING-----

This package is related to an experimental project and should be understood as not being necessarily useful (and even potentially quite dangerous, in terms of system stability/functionality) for regular users of Ubuntu and Ubuntu-based distributions.

If you know at least some amount about Debian packages, Debian packaging (and manually running apt & dpkg commands, for instance) then you might well see the uses of this project (enhancement to the apt-cache command for instance).

If you know a good amount about Debian packages, Debian packaging and perhaps even write complex combinations of apt and/or dpkg(-deb) commands in arcane Bash scripts AND want a to-your-needs custom-built Xenial-based Ubuntu (sort of) "rolling release" system, then dive on in!

For users whose knowledge (relative Debian/Ubuntu packaging and/or any Linux based distribution packaging) lies in between (or experts with knowledge far beyond) the two above stated broad groups of users: it is up to you if you want to experiment with this package and/or the procedures at the website.

*** The entire responsibility for any system breakage lies with the person(s) who use this package and/or the info/scripts given at the linked website. ***

So it's on you if you break it folks! And if that (perhaps) unfortunately happens, it's also a chance to learn potentially plenty about how to fix it (a big part of this whole process for me, for sure).

*** No warranty given, no liability whatsoever for me, so on, so forth. ***

-----END EXPERIMENTAL WARNING-----

This is a small package that adds the full set of standard repositories for six other Ubuntu "series" to the installed system, as well as a small bash script called "apt-all" that allows usage of "sudo apt-get install" commands relative the repositories of any of the six other series.

New "<series>-repositories-lo.pref" & "<series>-repositories-hi.pref.save" files (for the six other series) are created in /etc/apt/preferences.d (simple binary switch pinning system) and new "official-repos-<series>.list" files are created in /etc/apt/sources.list.d/ (identical to default "official-package-repositories.list" files for each series, renamed to "official-package-repos-<series>.list" so that all can co-exist on the one system). As stated, one small script "apt-all" is also added (to /usr/bin).

This package was designed and created on a Xenial-based system (Linux Mint 18.1 Serena MATE) with the intention of keeping Serena up-to-date far beyond the Xenial LTS EOL (the original package was called "serena-enhanced" on the test system used for all this work). MATE 1.16 based on GTK2 is great for me (MATE 1.18+ is now GTK3 based), as my consistent preference for more than 25 years of GUI-based computer operating system usage is for a simple, fast, visually uncluttered desktop experience.

Thus, it became important to be able to upgrade Serena (while keeping GTK2 based MATE 1.16) to Bionic-era (and newer) packages. The "ubuntu-enhanced" package in this PPA and this whole project is the result of that intention to make a Serena Enhanced (initially) and then more general Xenial Enhanced "rolling release" installation process.

Having seven series of packages and package information (specifically Xenial, Bionic, Cosmic, Disco, Eoan, Focal, and Groovy series) available can be very useful in various situations (ie. see the "apt-cache" section below). From ubuntu-enhanced v0.7 the package is installable on any of seven series. Based on the installed series, the installation process of ubuntu-enhanced will determine the six other series to add to the apt/dpkg system (with low pin priority being default) such that one can access packages (and package info) from all seven series.

*** Scripted pinning -- apt/dpkg ***

The "apt-all" script is key and if you take away the shebang + printargs + comments + whitespace (and compress a couple of commands to easy one-liners) it is about 30 lines of Bash commands.

What "apt-all" does is switch the name (using a ".save" filename extension) on the pin file (in /etc/apt/preferences.d) for one selected other series, giving that series temporary priority (990) over your own (500 by default). Then "apt-all" runs a standard "sudo apt-get install" command, thus giving the user access to all the packages in the single specified other series.

There are a few lines of code to monitor for a break signal to (try) ensure the file pinning a series other than that installed ends up with the default filename (ie. "<series>-repositories-hi.pref.save" in /etc/apt/preferences.d) once apt-all completes execution. So as to ensure other series usually have low priority (50) which is the default "state" for the setup (safe, no potential danger to system stability except if/when "apt-all" is invoked).

*** Power failures / why not sed / etc ***

Yes, sed could certainly be used to change the pin priority for the specified other series, but in the event of an entirely unexpected situation (ie. power failure, common in many parts of the world at least) you can end up with the other series still having high priority (on system restart and/or whenever next running an apt-get command).

So by using a simple renaming trick ("everything's a file" -- in this case, a set of files whose names show the toggle state for high pin preference of other series) in the event of power (or other) failure it is easy to see the current status. If any of the six "<series>-repositories-hi.pref.save" files in /etc/apt/preferences.d do NOT have a ".save" extension (and instead end only with ".pref") then something failed.

To fix a situation like this one simply needs to invoke "apt-all" again (no arguments necessary) as it always automatically renames any current hi pins (ie. "<series>-repositories-hi.pref" in /etc/apt/preferences.d) with correct ".save" extensions to clean-up after any possible previous failure.

There is also some error handling in "apt-all" relative a critical config file /etc/apt/otherseries that is created by ubuntu-enhanced on installation. That config file should ONLY have precisely six series names (other than your own) in it. The sanity check in "apt-all" is likely not foolproof if you put unexpected data in /etc/apt/otherseries so best not to modify that file (there should be no need to in any case).

*** apt-cache becomes more powerful ***

Even if you never use the "apt-all" script to grab packages from series other than your own, having the additional other repository pref files installed in the /etc/apt/preferences.d directory with low pin value (50) makes the apt-cache commands more powerful. For instance:

$ apt-cache policy meld
meld:
  Installed: 3.20.2-1~16.04.sav0
  Candidate: 3.20.2-1~16.04.sav0
  Version table:
     3.20.2-2 50
         50 http://archive.ubuntu.org/ubuntu groovy/universe amd64 Packages
         50 http://archive.ubuntu.org/ubuntu groovy/universe i386 Packages
     3.20.2-1ubuntu1 50
         50 http://archive.ubuntu.org/ubuntu focal/universe amd64 Packages
         50 http://archive.ubuntu.org/ubuntu focal/universe i386 Packages
 *** 3.20.2-1~16.04.sav0 500
        500 http://ppa.launchpad.net/savoury1/gtk-xenial/ubuntu xenial/main a
        500 http://ppa.launchpad.net/savoury1/gtk-xenial/ubuntu xenial/main i
        100 /var/lib/dpkg/status
     3.20.1-1 50
         50 http://archive.ubuntu.org/ubuntu eoan/universe amd64 Packages
         50 http://archive.ubuntu.org/ubuntu eoan/universe i386 Packages
     3.20.0-2 50
         50 http://old-releases.ubuntu.com/ubuntu disco/universe amd64 Packag
         50 http://old-releases.ubuntu.com/ubuntu disco/universe i386 Package
     3.18.3-1+16.04.sav0 500
        500 http://ppa.launchpad.net/savoury1/utilities/ubuntu xenial/main am
        500 http://ppa.launchpad.net/savoury1/utilities/ubuntu xenial/main i3
     3.18.2-1 50
         50 http://old-releases.ubuntu.com/ubuntu cosmic/universe amd64 Packa
         50 http://old-releases.ubuntu.com/ubuntu cosmic/universe i386 Packag
     3.18.0-6 50
         50 http://archive.ubuntu.org/ubuntu bionic/universe amd64 Packages
         50 http://archive.ubuntu.org/ubuntu bionic/universe i386 Packages
     3.14.2-1 500
        500 http://archive.ubuntu.org/ubuntu xenial/universe amd64 Packages
        500 http://archive.ubuntu.org/ubuntu xenial/universe i386 Packages

This shows all meld versions from seven official series as well as from two Launchpad PPAs. The current installed package version is 3.20.2-1~16.04.sav0 which is from ppa:savoury1/gtk-xenial (GTK 3.22.30 and other upgrades, making Meld 3.20 buildable). There is also a version 3.18.3 for Xenial users who have not upgraded their GTK stack (Meld 3.18.3 is the highest possible with GTK < 3.20). All repos other than Xenial are currently pinned low (50) with Xenial having usual priority (500) as it is the installed series.

It's actually quite often very handy to have a fast terminal way to check the versions of any specified package(s) in the seven different Ubuntu series in question. As shown, this can be done very simply with "apt-cache policy" (on any of the seven series) after this ubuntu-enhanced package is installed.

*** Usage: apt-all ***

The "apt-all" script takes one series name (Xenial, Bionic, Cosmic, Disco, Eoan, Focal, or Groovy) as the first argument (in lowercase) then exactly "install" as the second argument (safety, no accidental or even intentional "apt-get upgrade" commands are allowed with a different series via "apt-all") and then the package(s) that you want to install as the third argument.

Example: apt-all bionic install meld

This invocation would do these tasks:

=> Switch Bionic to have temporary high priority

=> $ sudo apt-get install meld

=> Switch Bionic back to low priority as usual 

Thus "apt-all" is simple to use and is all about giving a front-end for easy choice from all the packages of six other series of Ubuntu than your own.

FINAL WARNING: While easy to use, "apt-all" is also very powerful (due it allowing install of any/all packages from series other than that installed) and could destroy the reliability/functionality of an existing install with careless usage (ie. upgrade/downgrade to newer/older packages without doing necessary homework about dependency/compatibility with all related packages, possibly causing unrecoverable system-breakage). So, PLEASE BE CAREFUL!!!

*** Updates ***

26/8/19 -- New package ubuntu-enhanced replaces xenial-enhanced and is compatible for installation on four different Ubuntu series (Xenial, Bionic, Cosmic, Disco and Eoan). Gives access to all packages (in all "pockets") in all of the repositories for these series, no matter what series is installed.

2/9/19 -- New PPA (now also called ubuntu-enhanced) and new release 0.5 with improvements to "apt-all" for better error handling.

26/9/19 -- Fixed issue with post installation script on Bionic and newer due official-packages-repositories.list file no longer being used. Enhanced apt setup on series newer than Xenial is now actually configured correctly!

12/5/20 -- Added Groovy Gorilla to configuration and uploaded new 0.7 package allowing installation on any of seven series (Xenial through Groovy). This version also uses the correct server for old series past end of support.
