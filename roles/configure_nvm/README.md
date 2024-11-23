Here’s a neatly formatted Markdown file for the `configure_nvm` role overview, which you can place in the `role_overview` directory of your Ansible project.

### `configure_nvm.md`

```markdown
# Configure NVM

## Overview
The **Configure NVM** role automates the installation and configuration of Node Version Manager (NVM) on Ubuntu systems. NVM allows users to easily install and manage multiple versions of Node.js, providing flexibility for different projects and environments.

## Responsibilities
- Downloads the NVM installation script from the official repository.
- Executes the installation script to set up NVM.
- Modifies the user's profile (e.g., `.zshrc`) to include NVM in the environment.
- Installs the latest Long Term Support (LTS) version of Node.js using NVM.

## Tasks
1. **Download NVM Install Script**: 
   - Uses the `get_url` module to download the NVM installation script.

   ```yaml
   - name: Download NVM install script
     ansible.builtin.get_url:
       url: https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh
       dest: /tmp/install_nvm.sh
       mode: '0755'
   ```

2. **Install NVM**: 
   - Executes the downloaded script to install NVM.

   ```yaml
   - name: Install NVM
     ansible.builtin.shell: |
       set -o pipefail
       /tmp/install_nvm.sh | bash
     args:
       creates: "{{ ansible_env.HOME }}/.nvm"
   ```

3. **Add NVM to Profile**: 
   - Appends necessary environment variables and commands to load NVM in the user's profile file (e.g., `.zshrc`).

   ```yaml
   - name: Add NVM to profile
     ansible.builtin.lineinfile:
       path: "{{ ansible_env.HOME }}/.zshrc"  # Change to .bashrc if using Bash
       line: |
         export NVM_DIR="$HOME/.nvm"
         [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
         [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion
       state: present
   ```

4. **Install Node.js LTS Using NVM**: 
   - Uses a shell command to source the profile and install the latest LTS version of Node.js.

   ```yaml
   - name: Install Node.js LTS using NVM
     ansible.builtin.shell: |
       source ~/.zshrc && nvm install --lts
     args:
       executable: /bin/zsh
       creates: "{{ ansible_env.HOME }}/.nvm/versions/node/$(nvm version-remote --lts)"
   ```

## Usage
This role can be included in your main playbook as follows:

```yaml
- name: Configure NVM for Node.js management
  hosts: all
  roles:
    - configure_nvm
```

## Requirements 
- Ansible must be installed on the control node.
- The target machine must be running Ubuntu and have internet access to download packages and scripts.
