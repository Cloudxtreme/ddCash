
Debian
====================
This directory contains files used to package ddCashd/ddCash-qt
for Debian-based Linux systems. If you compile ddCashd/ddCash-qt yourself, there are some useful files here.

## ddCash: URI support ##


ddCash-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install ddCash-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your ddCashqt binary to `/usr/bin`
and the `../../share/pixmaps/ddCash128.png` to `/usr/share/pixmaps`

ddCash-qt.protocol (KDE)

