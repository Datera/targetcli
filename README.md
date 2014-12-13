# TargetCLI

TargetCLI is the LIO commmand-line administration tool for managing the Linux
SCSI Target, and its third-party target fabric modules and backend storage
objects.

Based on RTSLib, it allows direct manipulation of all SCSI Target objects like
storage objects, SCSI targets, TPGs, LUNs and ACLs, as well as manage startup
system configuration for the SCSI Target subsystem.

TargetCLI can be used either as a regular CLI tool, one command at a time, or
as an interactive shell based on the python configshell CLI framework, with
full auto-complete support and inline documentation.

TargetCLI is part of the Linux Kernel's SCSI Target's userspace management
tools.

## Installation

TargetCLI is currently part of several Linux distributions. In most cases,
simply installing the version packaged by your favorite Linux distribution is
the best way to get it running.

## Migrating away from a targetcli < 3.0 setup

Prior to version 3.x, TargetCLI relied on lio-utils for managing the target's
startup configuration. Unfortunately, rtslib.Config - now used by targetcli and
the `/etc/init.d/target` initscript for startup config save and restore
operations - is incompatible with the legacy lio-utils config files.

However, the new initscript has a special provision for this case. 
When attempting to start the target service when there is no
`/etc/target/scsi_target.lio` configuration file present, a check is made to see
if there is a target configuration currently running on the system. If there is,
it is assumed to be a keeper, and the initscript will attempt to dump it to the
system startup configuration file `/etc/target/scsi_target.lio`.

When migrating from a lio-utils install, the trick is to prevent the old lio-utils
package removal from stopping the service. For this, you can simply empty the
lio-utils version of `/etc/init.d/target` - or the equivalent location for your
Linux distribution.

Example on Debian:

        echo > /etc/init.d/target
        dpkg --purge lio-utils
        apt-get install targetcli

## Building from source

The packages are very easy to build and install from source as long as
you're familiar with your Linux Distribution's package manager:

1.  Clone the github repository for TargetCLI using `git clone
    https://github.com/Datera/targetcli.git`.

2.  Make sure build dependencies are installed. To build TargetCLI, you will need:

	* GNU Make.
	* python 2.6 or 2.7
	* A few python libraries: rtslib, configshell, lio-utils
	* Your favorite distribution's package developement tools, like rpm for
	  Redhat-based systems or dpkg-dev and debhelper for Debian systems.

3.  From the cloned git repository, run `make deb` to generate a Debian
    package, or `make rpm` for a Redhat package.

4.  The newly built packages will be generated in the `dist/` directory.

5.  To cleanup the repository, use `make clean` or `make cleanall` which also
    removes `dist/*` files.

## Documentation

A manpage is provided with this packages, simply use `man targetcli` to get
more information.

An other good source of information is the http://linux-iscsi.org wiki,
offering many resources such as a the TargetCLI User's Guide, online at
http://linux-iscsi.org/wiki/targetcli.

## Mailing-list

All contributions, suggestions and bugfixes are welcome!

To report a bug, submit a patch or simply stay up-to-date on the Linux SCSI
Target developments, you can subscribe to the Linux Kernel SCSI Target
development mailing-list by sending an email message containing only
`subscribe target-devel` to <mailto:majordomo@vger.kernel.org>

The archives of this mailing-list can be found online at
http://dir.gmane.org/gmane.linux.scsi.target.devel

## Author

LIO was developed by Datera, Inc.
http://www.datera.io

The original author and current maintainer is
Jerome Martin <jxm@netiant.com>
