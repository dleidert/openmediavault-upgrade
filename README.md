# openmediavault-upgrade

script to upgrade openmediavault from version 2 (Stoneburner) to version 3
(Erasmus)

## Preparations

I recommend to stop all services openmediavault provides, including Samba,
Rsync, Nfs, etc. **except SSH**. Just log into the GUI and disable these
services.  The rationale is to prevent your users from using these services
while the upgrade is running: Services might not be reliable during the
upgrade. Further the script will attempt to remount all data points as
read-only during the upgrade. This will fail if these services are still
accessing the data points in _/media/_.

The script also only covers basic OMV installations. If you use encryption,
graphical desktop environments, or if you have unusual boot screens you might
want to read the [Release Notes for Jessie][relnotes]. You might find the
chapters 4 and 5 as well as appendix A most important to read.

[relnotes]: https://www.debian.org/releases/jessie/amd64/release-notes/ch-information.en.html#openssh

## Usage

Login to the system locally or remotely (via SSH). Download the tarball, unpack
it, and run the script as root (or via _sudo_):

```
wget -qO- https://github.com/dleidert/openmediavault-upgrade/archive/2.0.tar.gz | tar -xz
cd openmediavault-upgrade-2.0
sudo ./omv-release-upgrade-3
```

The scripts will run without asking you back. They will exit if an error
occurs. If everything goes well you will be presented a message to restart your
system. Make sure to empty the browser cache before reloading the web site.

## Special hints

The scripts will install the latest kernel 4.9 available via security updates.

## License

The scripts are licensed under the GPL v3.
