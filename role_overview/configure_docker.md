Here’s a README file for the `install_docker` role, formatted similarly to your previous example for the `Configure Terminator` role.

### `install_docker.md`

```markdown
# Install Docker

## Overview
The **Install Docker** role automates the installation and configuration of Docker on Ubuntu systems. Docker is a platform that enables developers to automate the deployment of applications inside lightweight containers, providing a consistent environment across different stages of development.

## Responsibilities
- Installs required dependency packages necessary for Docker installation.
- Downloads and installs the Docker GPG key to ensure package integrity.
- Adds the Docker repository to APT sources for package management.
- Installs Docker Engine and related packages.
- Enables and starts the Docker service to ensure it runs on system boot.
- Adds the current user to the Docker group, allowing non-sudo access to Docker commands.

## Tasks
1. **Install Required Dependency Packages**: 
   - Uses the `apt` module to install necessary packages for Docker installation.

   ```yaml
   - name: Install required dependency packages
     ansible.builtin.apt:
       name:
         - apt-transport-https
         - ca-certificates
         - curl
         - software-properties-common
       state: present
       update_cache: true
     become: true
   ```

2. **Download Docker GPG Key**: 
   - Downloads the GPG key for verifying Docker packages.

   ```yaml
   - name: Download Docker GPG key  # noqa: command-instead-of-module
     ansible.builtin.command: >
       curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
     args:
       creates: /etc/apt/keyrings/docker.gpg
     become: true
   ```

3. **Add Docker Repository to APT Sources**: 
   - Adds the official Docker repository to APT sources.

   ```yaml
   - name: Add Docker repository to APT sources
     ansible.builtin.apt_repository:
       repo: >
         deb [arch={{ ansible_architecture }} signed-by=/etc/apt/keyrings/docker.gpg]
         https://download.docker.com/linux/ubuntu {{ ansible_distribution_release }} stable
       state: present
       filename: docker
     become: true
   ```

4. **Update Package Index**: 
   - Updates the package index after adding the new repository.

   ```yaml
   - name: Update package index
     ansible.builtin.apt:
       update_cache: true
     become: true
   ```

5. **Install Docker and Related Packages**: 
   - Installs Docker Engine and other related packages.

   ```yaml
   - name: Install Docker and related packages
     ansible.builtin.apt:
       name:
         - docker-ce
         - docker-ce-cli
         - containerd.io
         - docker-buildx-plugin
         - docker-compose-plugin
       state: present
     become: true
   ```

6. **Enable Docker Service to Start on Boot**:
   - Ensures that the Docker service starts automatically when the system boots.

   ```yaml
   - name: Enable Docker service to start on boot
     ansible.builtin.systemd:
       name: docker
       enabled: true
     become: true
   ```

7. **Start Docker Service**:
   - Starts the Docker service immediately after installation.

   ```yaml
   - name: Start Docker service
     ansible.builtin.systemd:
       name: docker
       state: started
     become: true
   ```

8. **Add User To Docker Group**:
   - Adds the current user to the `docker` group, allowing them to run Docker commands without sudo.

   ```yaml
   - name: Add User To Docker Group
     ansible.builtin.user:
       name: "{{ ansible_env.USER }}"
       groups: docker
       append: true
     become: true
   ```

## Usage
This role can be included in your main playbook as follows:

```yaml
- name: Install Docker on Ubuntu systems
  hosts: all
  roles:
    - install_docker
```

## Requirements 
- Ansible must be installed on the control node.
- The target machine must be running Ubuntu and have internet access to download packages.
