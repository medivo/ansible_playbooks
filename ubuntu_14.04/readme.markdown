ubuntu 14.04
===

updates & upgrades
---
Occasionally, ubuntu will print out the following message after you ssh in. The number of packages will vary.

```bash
57 packages can be updated.
24 updates are security updates.
```

To update and upgrade the packages, run the following commands, omit the pound sign and everything to the right of it.

```bash
sudo apt-get update        # Fetches the list of available updates
sudo apt-get upgrade       # Strictly upgrades the current packages
sudo apt-get dist-upgrade  # Installs updates (new ones)
```

For the packages to take effect, you must reboot your machine afterwards.
