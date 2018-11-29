Sample init scripts and service configuration for ddcashd
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/ddcashd.service:    systemd service unit configuration
    contrib/init/ddcashd.openrc:     OpenRC compatible SysV style init script
    contrib/init/ddcashd.openrcconf: OpenRC conf.d file
    contrib/init/ddcashd.conf:       Upstart service configuration file
    contrib/init/ddcashd.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "ddcash" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, ddcashd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, ddcashd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that ddcashd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If ddcashd is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/ddcash/ddcash.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/ddcash.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/ddcashd
Configuration file:  /etc/ddcash/ddcash.conf
Data directory:      /var/lib/ddcashd
PID file:            /var/run/ddcashd/ddcashd.pid (OpenRC and Upstart)
                     /var/lib/ddcashd/ddcashd.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the ddcash user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
ddcash user and group.  Access to ddcash-cli and other ddcashd rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start ddcashd" and to enable for system startup run
"systemctl enable ddcashd"

4b) OpenRC

Rename ddcashd.openrc to ddcashd and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/ddcashd start" and configure it to run on startup with
"rc-update add ddcashd"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop ddcashd.conf in /etc/init.  Test by running "service ddcashd start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy ddcashd.init to /etc/init.d/ddcashd. Test by running "service ddcashd start".

Using this script, you can adjust the path and flags to the ddcashd program by
setting the ddCashD and FLAGS environment variables in the file
/etc/sysconfig/ddcashd. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
