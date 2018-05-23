# Installing Applications in Linux

## Package Managers

An application which manages the installed packaged on the system.
A Linux distribution has one package manager at its core.

Distribution	Package manager	File type	Local command
Debian	apt	.deb	dpkg
RedHat	yum	.rpm	rpm
Arch	pacman
SUSE	zypper	.rpm

## dpkg

-i for install
takes only .deb files

## Porting Packages

Converting from rpm to deb is possible using the Alien tool from the Universe repository.

1. apt install alien
2. alien package.rpm

The result is a deb package.

Alien also allows directly installing the rpm package. `alien -i package.rpm`
The latter is not advised as this can result independency issues. These dependencies then need to be converted as well, resulting in more chance at broken dependencies.
A better approach would be to install the software from source code.

## From source

