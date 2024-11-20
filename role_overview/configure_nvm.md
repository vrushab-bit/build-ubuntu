# Configure NVM Role

## Overview
The **configure_nvm** role installs and configures Node Version Manager (NVM) on Ubuntu systems. This role allows users to easily manage multiple versions of Node.js, enhancing flexibility in development environments.

## Responsibilities
- Installs NVM using a shell script.
- Installs the latest LTS version of Node.js or a specified version.
- Sets up environment variables in the user's shell profile to ensure NVM is available in future sessions.

## Tasks
- Uses the `ansible.builtin.get_url` module to download the NVM install script:
  ```yaml
  - name: Download NVM install script
    ansible.builtin.get_url:
      url: https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh
      dest: /tmp/install_nvm.sh
      mode: '0755'
