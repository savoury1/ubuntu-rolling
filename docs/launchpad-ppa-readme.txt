See https://github.com/savoury1/ubuntu-rolling for detailed procedures relating to this package and how to (even significantly) upgrade your own Xenial-based system with packages from newer than Xenial series of Ubuntu.

UPDATE (2/9/19): New PPA (now also called ubuntu-enhanced) and new release 0.5 with improvements to "apt-all" for better error handling.

UPDATE (26/8/19): New package ubuntu-enhanced replaces xenial-enhanced and is compatible for installation on five different Ubuntu series (Xenial, Bionic, Cosmic, Disco and Eoan). Gives access to all packages (in all "pockets") in all of the repositories for these series, no matter what series is installed.

-----START EXPERIMENTAL WARNING-----

This package is related to an experimental project and should be understood as not being necessarily useful (and even potentially highly dangerous, in terms of system stability/functionality) for regular users of Ubuntu and Ubuntu-based distributions.

There is _intentionally_ a lot of text in this description to give anyone arriving here a moment of pause, before heading down to the bottom of the page to grab the commands for adding this PPA to your own system (so if you tell readers the direct commands to add this PPA on your blog/site, please also firstly print a link to this exact description page and warn users to have a read before proceeding).

If you know at least some amount about Debian packages, Debian packaging (and manually running apt & dpkg commands, for instance) then you will likely be able to see and understand at least some of what this project is doing.

If you know a good amount about Debian packages, Debian packaging and perhaps even write complex combinations of apt and/or dpkg(-deb) commands in arcane Bash scripts AND want a to-your-needs custom-built Xenial-based Ubuntu (sort of) "rolling release" system, then dive on in!

For users whose knowledge (specifically relative Debian/Ubuntu packaging and/or any Linux based distribution packaging) lies in between (or experts with knowledge far beyond) those two above stated broad groups of user: it is up to you if you want to experiment with this package and/or the procedures at the website.

*** The entire responsibility for any system breakage lies with the person(s) who use this package and/or the info/scripts given at the linked website. ***

So it's on you if you break it folks! And if that (perhaps) unfortunately happens, it's also a chance to learn potentially plenty about how to fix it (a big part of this whole process for me, for sure).

*** No warranty given, no liability whatsoever for me, so on, so forth. ***

-----END EXPERIMENTAL WARNING-----

This is a small package that adds the full set of standard repositories for four other Ubuntu "series" to the installed system, as well as a small bash script called "apt-all" that allows you to use "sudo apt-get install" commands with the repositories of any of the four other series.

New "<series>-repositories-lo.pref" & "<series>-repositories-hi.pref.save" files (for the four other series) are created in /etc/apt/preferences.d (simple binary switch pinning system) and new "official-repos-<series>.list" files are created in /etc/apt/sources.list.d/ (identical to default "official-package-repositories.list" files for each series, renamed to "official-package-repos-<series>.list" so that all can co-exist on the one system). As stated, one small script "apt-all" is also added (to /usr/bin).

This package was designed and created on a Xenial-based system (Linux Mint 18.1 Serena MATE) with the intention of keeping Serena up-to-date far beyond the Xenial LTS EOL (the original package was called "serena-enhanced" on the test system used for all this work). MATE 1.16 based on GTK2 is great for me (MATE 1.18+ is now GTK3 based), as my consistent preference for more than 25 years of GUI-based operating system usage is for a simple, fast, visually uncluttered desktop experience.

Thus, it became important to be able to upgrade Serena (while keeping GTK2 based MATE 1.16) to Bionic-era (and newer) packages. The "ubuntu-enhanced" package in this PPA and this whole project is the result of that intention to make a Serena Enhanced "rolling release" installation.

To make the usefulness of accessing package lists from five possible series (ie. Xenial, Bionic, Cosmic, Disco and Eoan) available to users with newer than Xenial installs (ie. see the "apt-cache" section below) the package has been upgraded (v0.4) to be compatible for installation on any of the five mentioned series. So no matter which of the five series is running, the installation process of ubuntu-enhanced will determine the four other series to add to the apt/dpkg system (with low pin priority being default).

*** Scripted pinning -- apt/dpkg ***

The "apt-all" script is key and if you take away the shebang + printargs + comments + whitespace (and compress a couple of commands to easy one-liners) it is about 30 lines of Bash commands.

All "apt-all" does is switch the name (using a ".save" filename extension) on the pin file (in /etc/apt/preferences.d) for the selected other series, giving that series temporary priority (990) over your own (500 by default). Then "apt-all" runs a standard "sudo apt-get install" command, thus giving the user access to all the packages in the single specified other series.

There are a few lines of code to monitor for a break signal to (try) ensure the file pinning a series other than that installed ends up with the default filename (ie. "<series>-repositories-hi.pref.save" in /etc/apt/preferences.d) once apt-all completes execution. So as to ensure other series usually have low priority (50) which is the default "state" for the setup (safe, no potential danger to system stability except if/when "apt-all" is invoked).

*** Power failures / why not sed / etc ***

Yes, sed could certainly be used to change the pin priority for the specified other series, but in the event of an entirely unexpected situation (ie. power failure, common in many parts of the world at least) you can end up with the other series still having high priority (on system restart and/or whenever next running an apt-get command).

So by using a simple renaming trick ("everything's a file" -- in this case, a set of files whose names show the toggle state for high pin preference of other series) in the event of power (or other) failure it is easy to see the current status. If any of the four "<series>-repositories-hi.pref.save" files in /etc/apt/preferences.d do NOT have a ".save" extension (and instead end only with ".pref") then something failed.

To fix a situation like this one simply needs to invoke "apt-all" again (no arguments necessary) as it always automatically renames any current hi pins (ie. "<series>-repositories-hi.pref" in /etc/apt/preferences.d) with correct ".save" extensions to clean-up after any possible previous failure.

There is also some error handling in "apt-all" relative a critical config file /etc/apt/otherseries that is created by ubuntu-enhanced on installation. That config file should ONLY have precisely four series names (other than your own) in it. The sanity check in "apt-all" is likely not foolproof if you put unexpected data in /etc/apt/otherseries so best not to modify that file (there should be no need to in any case).

*** apt-cache becomes more powerful ***

Even if you never use the "apt-all" script to grab packages from series other than your own, having the additional other repository pref files installed in the /etc/apt/preferences.d directory with low pin value (50) makes the apt-cache commands more powerful. For instance:

  $ apt-cache policy meld
  meld:
    Installed: 3.20.0-2
    Candidate: 3.20.0-2
    Version table:
   *** 3.20.0-2 100
           50 http://archive.ubuntu.com/ubuntu disco/universe amd64 Packages
           50 http://archive.ubuntu.com/ubuntu disco/universe i386 Packages
           50 http://archive.ubuntu.com/ubuntu eoan/universe amd64 Packages
           50 http://archive.ubuntu.com/ubuntu eoan/universe i386 Packages
          100 /var/lib/dpkg/status
       3.18.2-1 50
           50 http://archive.ubuntu.com/ubuntu cosmic/universe amd64 Packages
           50 http://archive.ubuntu.com/ubuntu cosmic/universe i386 Packages
       3.18.0-6 50
           50 http://archive.ubuntu.com/ubuntu bionic/universe amd64 Packages
           50 http://archive.ubuntu.com/ubuntu bionic/universe i386 Packages
       3.14.2-1 500
          500 http://archive.ubuntu.com/ubuntu xenial/universe amd64 Packages
          500 http://archive.ubuntu.com/ubuntu xenial/universe i386 Packages

This shows all versions of meld from five official series, revealing that the currently installed package version is evidently 3.20.0-2 from Disco/Eoan series (the same package is used for both those series, as revealed by the output of the apt-cache command). It also shows that all repos other than Xenial are currently pinned low (50) with Xenial having usual priority (500) as it is the installed series.

It's actually quite often very handy to have a fast terminal way to check the versions of any specified package(s) in the five different Ubuntu series in question. As shown, this can be done very simply with "apt-cache policy" (on any of the five series) after this ubuntu-enhanced package is installed.

*** Usage: apt-all ***

The "apt-all" script takes one series name (ie. Xenial, Bionic, Cosmic, Disco or Eoan) as the first argument (in lowercase) then exactly "install" as the second argument (safety, no accidental or even intentional "apt-get upgrade" commands are allowed with "apt-all"!) and then the package(s) that you want to install as the third argument.

Example: apt-all bionic install meld

This invocation would do these tasks:

=> Switch Bionic to have temporary high priority

=> $ sudo apt-get install meld

=> Switch Bionic back to low priority as usual 

So "apt-all" is simple to use and is all about giving a front-end for easy choice from all the packages of four other series of Ubuntu than your own.

FINAL WARNING: While easy to use, "apt-all" is also very powerful (due it allowing install of any/all packages from series other than current) and could destroy the reliability/functionality of an existing installation with careless usage (ie. upgrade/downgrade to newer/older packages without doing necessary homework about dependency/compatibility with other core packages, possibly causing unrecoverable system-breakage). PLEASE BE CAREFUL!!!
