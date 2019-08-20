## Some philosophical background

These procedures have specifically been created using a starting point of Linux Mint Serena 18.1 MATE (a Xenial-based Ubuntu distribution) as it has a GTK2 based desktop. Note the quote in the center of the screenshot below (MATE 1.16.2 "about" box):

> GNOME 2 was the most popular Linux desktop but it's no longer available...
> MATE is here to provide that same desktop to you!

Question: if GNOME 2 was the most popular Linux desktop, **why** is it no longer available?

Based on all testing of GNOME 3 it is overly complex, [bug-ridden](https://igurublog.wordpress.com/2012/11/05/gnome-et-al-rotting-in-threes/), [garbage](https://fosspost.org/opinions/are-gtk-developers-destroying-linux-desktop-with-their-plans) and yet this is the desktop base that Canonical have now chosen for Bionic and onwards! Even Clem (the main guy behind Linux Mint), who was known to rant about breakages caused by GNOME 3 and GTK3 a few years back, decided that from Mint 18.2 the MATE desktop would be based on GTK3!?!

Also, the obvious arrogance of the GNOME 3 developers is indicative of trends within the entire software industry (both proprietary and "free"). Those trends for some years now (at least with many software development teams, though certainly not all) are to consistently ignore user feedback, strip features from newer releases, change paradigms about how core parts of the system (such as the desktop environment) have worked for years, all because "I'm the developer and say it's better!" This "dumbing down" in the name of "progress" with rapidly released new versions all about "new features" (rather than less glamorous though much-needed bug-fixing) is actually destroying the reliability of computer software on the whole.

This coming from someone who has done paid tech support work since his mid-teens in the late 1980s (DOS based systems) and for 30 years since. So this includes with all iterations of Windows (to the level of Server Engineer designing and installing web server farms for global corporations, hosting sites with millions of hits per day), many iterations of Mac OS, many iterations of Linux, and even some genuine UNIX systems.

Personally, the long overdue switch from Windows to a Linux distribution as the day-to-day OS (prompted by the user-hostile, spyware ridden, forced updates garbage known as "Windows 10") happened at exactly the point when Mint 18.1 Serena had just been released. And it had a fast, usable GTK2 based desktop that was easy to configure and just works.

This was serendipitous timing. After Mint 18.2 MATE was released and having done some testing of that version (with the "new, improved" GTK3 based desktop garbage), it was clear that Mint 18.1 was going to be the way forward for a long time. Thus, a procedure had to be created to allow upgrading core "modules" or sub-systems of the OS, without touching the simple, fast, usable GTK2 based desktop. Hence, the Ubuntu (Xenial-based) "Rolling Release" system, specifically crafted on a Mint 18.1 Serena MATE installation.

![Serena Enhanced](images/Serena-Enhanced.png)