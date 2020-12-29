# openmediavault-upgrade

This repository contains scripts to non-interactively upgrade openmediavault
from one major release to the next. This includes upgrading the Debian Linux
operating system OMV is running on.

The individual scripts are in their respective branches:

OMV2 -> 3: <https://github.com/dleidert/openmediavault-upgrade/tree/2>

OMV3 -> 4: <https://github.com/dleidert/openmediavault-upgrade/tree/3>

OMV4 -> 5: ...to be written...

Every branch has its own `README` file with instructions. Basically just get
the [tarball] for your OMV version, unpack it, and run the
`omv-release-upgrade-X` script (where `X` marks the OMV version we are
upgrading to.

[tarball]: https://github.com/dleidert/openmediavault-upgrade/releases

It is also possible to get the most recent version by downloading a tarball
for a branch, e.g.:

```
wget -qO- https://github.com/dleidert/openmediavault-upgrade/tarball/3 | tar -xz
cd dleidert-openmediavault-upgrade-<hash>
sudo ./omv-release-upgrade-4
```

Feel free to report any issues via the [issue tracker]. Please include the log
file which is created by the script.

[issue tracker]: https://github.com/dleidert/openmediavault-upgrade/issues

The scripts are based on the original `omv-release-upgrade` script but are much
more extensive and protective. Still they won't cover all cases. Please make
sure to check out the _Debian release notes_ for your release upgrade.

The scripts are licensed under the [GPL-3](LICENSE) too.

<!-- vim: set tw=79 ts=2 sw=2 ai si et: -->
