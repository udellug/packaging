# Packaging

## What is a Package? Package Manager?

A package is just a neatly put together bundle of software.

A package manager is a something that makes it nice to install packages.

**Compare/contrast Windows software installation with apt installation**

## Overview of Debian's Role

Talking about Debian package management, because it would be impossible to
address every case across every distro.

![image](/home/ryan/repos/packaging-demo/package_manager_comparison_table.jpg)

![Linux Distro Timeline](https://www.cyberciti.biz/tips/wp-content/uploads/2007/06/44218-linuxdistrotimeline-7.2.png)

![Debian Family Tree](https://upload.wikimedia.org/wikipedia/commons/d/d8/Debian_family_tree_11-06.png)

### dpkg
dpkg started as a shell script to install packages, stands for Debian package.
It can be used to install, remove, and keep track of packages (among other
things).

It also makes sure that dependencies of software you install are satisfied.

### apt
Debian developed the Advanced Package Tool, or APT to make installing and
managing software simpler. It automatically gets packages from predefined
repositories, making your life blissfully simple.

### Teamwork makes the dreamwork - installing a package
1. apt will check for the packages dependencies and ask you if it's okay to
   install them
2. apt will download the packages, make sure they're correct (with md5sums), and
   then tell dpkg to install them.
3. dpkg will extract the archive and move the files to their correct locations
4. run {pre,post}{inst,rm} scripts (more later)
5. execute any actions based on triggers, after all packages are done, so that
   things like updating grub aren't done many times when you really only want to
   do it once. Also updates bash-completion, update-initramfs, and
   [more](https://wiki.debian.org/DpkgTriggers)

## What's In a Package?

What happens when you `sudo apt install package`

it gets a .deb file from a server SOMEWHERE (discussed later)

deb - Debian binary package format, an `ar` archive

`sudo apt-get --download-only install tree` will download the file to
/var/cache/apt/archives

`ar -t tree_1.7.0-5_amd64.deb` will list files

### debian-binary

debian-binary is the version of .deb package, so you'd be hard pressed to find
a package that's at least semi modern without a `2.0` for the content. If it's
not version 2, the package won't be installed.

### control.tar.gz

control.tar.gz has at least two files
 - control
    Information about package name, version, maintainer, installation size,
    dependencies, homepage, and description
 - md5sums
    md5sums for each file included in the package, so you can think you're safe

It can also have configuration files, {pre,post}{inst,rm} scripts, and some
other stuff.

This data can be viewed through the command `dpkg --info tree_1.7.0-5_amd64.deb`

### data.tar.xz

data.tar.xz has the actual package, with the folder structure that they should
have when installed on the system.

## Making Packages

[https://www.debian.org/doc/manuals/debmake-doc/ch04.en.html](https://www.debian.org/doc/manuals/debmake-doc/ch04.en.html)


## Where Do Packages Come From?

Any given distro will have servers that hold all the packages they provide to
users. Ubuntu systems will keep a list of those servers at
`/etc/apt/sources.list`.

The four main repositories are:

1. Main - Canonical-supported free and open-source software.
2. Universe - Community-maintained free and open-source software.
3. Restricted - Proprietary drivers for devices.
4. Multiverse - Software restricted by copyright or legal issues.

[Check where a specific package comes
from](https://askubuntu.com/questions/8560/how-do-i-find-out-which-repository-a-package-comes-from#8567)

So the packages I use are here
[http://us.archive.ubuntu.com/ubuntu/](http://us.archive.ubuntu.com/ubuntu/)

## Where Do Packages Go? Where Do Packages Get Uploaded to Cotton Eye Joe?

[Including this because I had a really hard time finding much of any information
about
it.](https://askubuntu.com/questions/16446/how-to-get-my-software-into-ubuntu)

## Future: Universal Packages

Snappy - a platform-agnostic package management system, theoretically.
Security-minded through containerization, theoretically. Developed by Canonical
(creators of Ubuntu).

AppImage - Distributing portable software without needing root priveledge to
install software. Download one file and run it. Basically mounts a compressed
image of the software when run.

<!-- vim: set tw=80 spell -->
