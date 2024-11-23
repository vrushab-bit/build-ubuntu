# Configure Visual Studio Code

## Overview
The **Configure Visual Studio Code** role automates the installation of Visual Studio Code (VS Code) on Ubuntu systems via Snap. VS Code is a popular source code editor that supports various programming languages and provides numerous extensions, including those for Ansible development.

## Responsibilities
- Installs Visual Studio Code using Snap if it is not already installed.
- Notifies the user if Visual Studio Code is already installed or if it has just been installed.

## Tasks
1. **Install Visual Studio Code via Snap**: 
   - Uses the `community.general.snap` module to install VS Code if it is not already present on the system.

   ```yaml
   - name: Install Visual Studio Code via Snap if not already installed
     community.general.snap:
       name: code
       state: present
     when:
       - snap_check.rc == 0
       - vscode_check.rc != 0
   ```

2. **Notify if Visual Studio Code is Already Installed**: 
   - Uses the `debug` module to inform the user that VS Code is already installed.

   ```yaml
   - name: Notify if Visual Studio Code is already installed
     ansible.builtin.debug:
       msg: "Visual Studio Code is already installed."
     when: vscode_check.rc == 0
   ```

3. **Notify if Visual Studio Code Was Just Installed**: 
   - Uses the `debug` module to inform the user that VS Code has been successfully installed via Snap.

   ```yaml
   - name: Notify if Visual Studio Code was just installed
     ansible.builtin.debug:
       msg: "Visual Studio Code has been installed via Snap."
     when:
       - snap_check.rc == 0
       - vscode_check.rc != 0
   ```

## Usage
This role can be included in your main playbook as follows:

```yaml
- name: Configure Visual Studio Code for users
  hosts: all
  roles:
    - configure_vscode
```

## Requirements 
- Ansible must be installed on the control node.
- The target machine must be running Ubuntu and have internet access to download packages.
