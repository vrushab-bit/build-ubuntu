# Configure Terminator

## Overview
The **Configure Terminator** role installs and configures the Terminator terminal emulator on Ubuntu systems. Terminator allows users to manage multiple terminal sessions in a single window, enhancing productivity through its advanced features.

## Responsibilities
- Installs Terminator using the package manager.
- Copies a pre-defined configuration file to the user's home directory, ensuring that Terminator starts with user-specific settings.

## Tasks
- Uses the `apt` module to install Terminator:
  ```yaml
  - name: Install Terminator
    apt:
      name: terminator
      state: present
  - name: Copy Terminator config file
    copy:
      src: files/config   # Path to your config file in the role's files directory
      dest: ~/.config/terminator/config
      owner: "{{ ansible_user }}"
      mode: '0644'