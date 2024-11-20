# Configure VSCode and Extensions

## Overview
The **Configure VSCode** role installs and configures Visual Studio Code (VSCode) on Ubuntu systems. This role enhances the development environment by installing essential extensions and applying user-specific settings, thereby improving productivity.

## Responsibilities
- Installs Visual Studio Code using Snap.
- Installs a set of predefined extensions for VSCode.
- Copies a pre-defined configuration file to the user's settings directory, ensuring that VSCode starts with user-specific preferences.

## Tasks
- Uses the `community.general.snap` module to install Visual Studio Code:
  ```yaml
  - name: Install Visual Studio Code via Snap if not already installed
    community.general.snap:
      name: code
      state: present
  ```

- Loads a list of VSCode extensions from a separate YAML file and installs them:
  ```yaml
  - name: Load VSCode extensions from file
    ansible.builtin.include_vars:
      file: "{{ playbook_dir }}/roles/configure_vscode/files/extensions.yml"
      name: vscode_exts

  - name: Install Extensions for VSCode
    ansible.builtin.command:
      cmd: code --install-extension {{ item }} --force
      creates: "{{ ansible_env.HOME }}/.vscode/extensions/{{ item }}"
    loop: "{{ vscode_exts.extensions }}"
  ```

- Copies a custom `settings.json` file to the user's settings directory:
  ```yaml
  - name: Copy custom settings.json to user directory
    ansible.builtin.template:
      src: settings.json.j2         # Path to your custom settings template file
      dest: "{{ ansible_env.HOME }}/.config/Code/User/settings.json"
      mode: "0644"
  ```

## Example Usage

To use this role in your playbook, include it as follows:

```yaml
- hosts: localhost
  become: yes
  roles:
    - configure_vscode
```

### Extensions File

Make sure to define your desired extensions in the `extensions.yml` file located in the `files` directory of the role:

```yaml
extensions:
  - ms-python.python           # Python extension
  - ms-vscode.cpptools         # C/C++ extension
  - esbenp.prettier-vscode      # Prettier extension
  - dbaeumer.vscode-eslint      # ESLint extension
```

### Custom Settings

Customize your VSCode settings by modifying the `settings.json.j2` template located in the `templates` directory of the role.

