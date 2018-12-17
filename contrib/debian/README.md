
Debian
====================
This directory contains files used to package adcoind/adcoin-qt
for Debian-based Linux systems. If you compile adcoind/adcoin-qt yourself, there are some useful files here.

## adcoin: URI support ##


adcoin-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install adcoin-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your adcoinqt binary to `/usr/bin`
and the `../../share/pixmaps/adcoin128.png` to `/usr/share/pixmaps`

adcoin-qt.protocol (KDE)

