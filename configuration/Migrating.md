# Migrating to Network Manager based configure.py

## Install Dependencies

Run `sudo apt-get install network-manager python-networkmanager`

Reboot to make sure that dbus and network-manager are initialized.

## Add Policykit File

Create `/etc/polkit-1/localauthority/50-local.d/nm.pkla`

Add this in the file:
```
[network-manager]
Identity=unix-group:netdev
Action=org.freedesktop.NetworkManager.*
ResultAny=yes
ResultInactive=no
ResultActive=yes
```

Add your user to the netdev group by running
`sudo addgroup $USER netdev`

## Remove Old stuff from interfaces

Open `/etc/network/interfaces` with sudo and your favorite editor

Replace all the contents with this, otherwise network-manager doesn't 
actually do anything.

```
# The loopback network interface
auto lo
iface lo inet loopback

allow-hotplug eth0
iface eth0 inet dhcp

```