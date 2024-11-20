# Update System Packages

## Overview
The **Update System Packages** role is designed to ensure that all system packages on an Ubuntu installation are up to date. This role helps maintain system security and stability by applying the latest updates and upgrades.

## Responsibilities
- Updates the package list to retrieve the latest version of available packages.
- Upgrades all installed packages to their latest versions.

## Tasks
- Uses the `apt` module to perform:
  - `apt update`: Refreshes the list of available packages.
  - `apt upgrade`: Installs the newest versions of all packages currently installed on the system.

