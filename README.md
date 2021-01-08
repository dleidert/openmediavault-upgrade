# omv-release-upgrade-5

script to upgrade openmediavault from version 4 (Arrakis) to version 5
(Usul)

## Preambel

These scripts are not provided by the Openmediavault project. They are solely
based on my knowledge of several successful upgrades and are tested in a VM.

The script only covers "basic" OMV installations. If you use encryption,
customized boot screens, graphical desktop environments, third party
repositories, if you heavily tempered with the system, etc. you might want to
**read the [Release Notes for Buster][relnotes]**. You might find the
**chapters 4 and 5** as well as **appendix A** most important to read.

[relnotes]: https://www.debian.org/releases/buster/amd64/release-notes/ch-information.en.html#openssh

## Preparations

I recommend to stop all services openmediavault provides, including Samba,
Rsync, Nfs, etc. **except SSH**. Just log into the GUI and disable these
services.  The rationale is to prevent your users from using these services
while the upgrade is running: Services might not be reliable during the
upgrade. Further the script will attempt to remount all data points as
read-only during the upgrade. This will fail if these services are still
accessing the data points in `/media/`.

If the script is executed on a remote system (e.g. over SSH) I highly recommend
to use the [`screen`](https://packages.debian.org/screen) utility. Get it via:

```
sudo apt-get install -y screen
```

`screen` has the advantage that even if you loose the SSH connection - which
will close the running shell together with all scripts and programs started
from this shell - you can always login again and re-attach to the running
`screen` session.

### Plexmediaserver

In OMV5 the `openmediavault-plexmediaserver` has been removed. The PLEX media
server is supposed to run via docker/portainer. Thus the `plexmediaserver`
package will be removed together with the plugin except you have enabled the
plex repository in `/etc/apt/sources.list.d/plexmediaserver.list` and installed
the key as described in the file:

```
# When enabling this repo please remember to add the PlexPublic.Key into the apt setup.
# wget -q https://downloads.plex.tv/plex-keys/PlexSign.key -O - | sudo apt-key add -
deb https://downloads.plex.tv/repo/deb/ public main
```

The scripts can do this for you. In [`inc/envvars`](inc/envvars) uncomment the
following line and set the variable to *anything other than `0`*:

```
export THIS_UPGRADE_KEEP_PLEX=1
```

## Usage

Login to the system locally or remotely (via SSH).

### Preparing `screen` session

If you have downloaded `screen` we create a session with the name `mysession`
and log into it:

```
screen -dmS mysession
screen -d -r mysession
```

You can always detach from the session pressing `<CTRL>+<a>` followed by `<d>`.
To reattach to the session use the command `screen -d -r mysession`. To end the
session type `exit` in the screen console. There are excellent howtos out there.

### Getting and running the upgrade script

Download the tarball, unpack it, and run the script as root (or via `sudo`):

```
wget -qO- https://github.com/dleidert/openmediavault-upgrade/archive/v4.5.tar.gz | tar -xz
cd openmediavault-upgrade-4.5
sudo ./omv-release-upgrade-5
```

The scripts will run without asking you back but all the output is stored in a
local logfile called

`omv_release_upgrade_to_buster.<date_and_time>.log`

If an error occurs the script will stop. In this case add the log file if you
seek support in the [forum] or in the [GitHub issue tracker].

If everything goes well you will be presented a message to restart your system.
Make sure to empty the browser cache before reloading the web site. Check that
all the services you need get enabled again or have been restarted.

[forum]: https://forum.openmediavault.org/
[GitHub issue tracker]: https://github.com/dleidert/openmediavault-upgrade/labels/omv-4.x

### Special hints

#### Chronyd

For the upgrade to succeed it is vital to disable the seccomp filter for the
`chronyd` service on systems having seccomp support disabled at the time of
upgrade. If the upgraded kernel has support for seccomp:

```
sudo modprobe -q configs
zgrep CONFIG_SECCOMP /boot/config-$(uname -r) /proc/config*
CONFIG_SECCOMP=y
CONFIG_SECCOMP_FILTER=y
```

(the **y** says it is enabled) you can restore running chronyd with seccomp
rules by editing `/etc/default/chrony` and setting the `DAEMON_OPTS` back to:

```
DAEMON_OPTS="-F -1"
```

#### Weg GUI

When re-entering the web GUI of OMV you might be presented a request to apply
your changes. Just follow the request.

## License

The scripts are licensed under the GPL v3.
