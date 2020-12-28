# openmediavault-upgrade

script to upgrade openmediavault from version 2 (Stoneburner) to version 3
(Erasmus)

## Preambel

These scripts are not provided by the Openmediavault project. They are solely
based on my knowledge of several successful upgrades and are tested in a VM.

The script only covers "basic" OMV installations. If you use encryption,
customized boot screens, graphical desktop environments, third party
repositories, if you heavily tempered with the system, etc. you might want to
**read the [Release Notes for Jessie][relnotes]**. You might find the
**chapters 4 and 5** as well as **appendix A** most important to read.

[relnotes]: https://www.debian.org/releases/jessie/amd64/release-notes/ch-information.en.html#openssh

## Preparations

I recommend to stop all services openmediavault provides, including Samba,
Rsync, Nfs, etc. **except SSH**. Just log into the GUI and disable these
services.  The rationale is to prevent your users from using these services
while the upgrade is running: Services might not be reliable during the
upgrade. Further the script will attempt to remount all data points as
read-only during the upgrade. This will fail if these services are still
accessing the data points in `/media/`.

If the script is executed on remote system (e.g. over SSH) I highly recommend
to use the [`screen`][screen] utility. Get it for your architecture (`dpkg
--print-architecture`) from the previous link and install it. For the `amd64`
architecture it is:

```
wget http://snapshot.debian.org/archive/debian/20150907T042834Z/pool/main/s/screen/screen_4.1.0~20120320gitdb59704-7%2Bdeb7u1_amd64.deb
sudo dpkg -i screen_4.1.0~20120320gitdb59704-7+deb7u1_amd64.deb
```

`screen` has the advantage that even if you loose the SSH connection - which
will close the running shell together with all scripts and programs started
from this shell - you can always login again and re-attach to the running
`screen` session.

[screen]: http://snapshot.debian.org/package/screen/4.1.0~20120320gitdb59704-7%2Bdeb7u1/

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
wget -qO- https://github.com/dleidert/openmediavault-upgrade/archive/2.1.tar.gz | tar -xz
cd openmediavault-upgrade-2.1
sudo ./omv-release-upgrade-3
```

The scripts will run without asking you back but all the output is stored in a
local logfile called

`omv_release_upgrade_to_jessie.<date_and_time>.log`

If an error occurs the script will stop. In this case add the log file if you
seek support in the [forum] or in the [GitHub issue tracker].

If everything goes well you will be presented a message to restart your system.
Make sure to empty the browser cache before reloading the web site. Check that
all the services you need get enabled again or have been restarted.

[forum]: https://forum.openmediavault.org/
[GitHub issue tracker]: https://github.com/dleidert/openmediavault-upgrade/labels/omv-2.x

### Special hints

The scripts will install the latest kernel 4.9 available via security updates.

The `rsync` service might be shown as not running although enabled. In this
case disable and enable it via the GUI or use these commands:

```
sudo systemctl enable rsync
sudo systemctl start rsync
sudo service rsync start
```

## License

The scripts are licensed under the GPL v3.
