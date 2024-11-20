# Ansible Automation Script for Ubuntu Customization
[![Technology](https://skillicons.dev/icons?i=ansible,ubuntu,linux&theme=light)](https://skillicons.dev)

### Description
This project provides an Ansible automation script designed to streamline the customization of Ubuntu installations. The script installs and configures all necessary packages and applications, setting up various profiles on the system to enhance productivity and user experience.

### Roles Overview
The script currently includes two primary roles:

- [Update System Packages](role_overview/update_system_packages.md): This role updates and upgrades system packages on Ubuntu.
- [Configure Terminator](role_overview/configure_terminator.md): This role installs Terminator and configures it with user preferences.


### Installation Instructions
To use this Ansible script, follow these steps:

1. **Prerequisites**
   - Ensure you have Ansible installed on your control machine. If not, you can install it by following these commands:
     ```bash
     sudo apt update
     sudo apt install ansible
     ```

2. **Clone the Repository**
   - Clone this repository to your local machine:
     ```bash
     git clone https://github.com/vrushab-bit/build-ubuntu.git
     cd build-ubuntu
     ```

3. **Run the Playbook**
   - Execute the Ansible playbook to apply the roles:
     ```bash
     ansible-playbook main.yml --ask-become-pass
     ```
   - The `--ask-become-pass` flag prompts for your sudo password, as elevated privileges are required for package installation.

### Directory Structure
The project follows a standard Ansible directory structure:

```
build-ubuntu/
├── main.yml                  # Main playbook file
├── roles/
│   ├── update_system_packages/
│   │   ├── tasks/
│   │   │   └── main.yml      # Tasks for updating system packages
│   └── configure_terminator/
│       ├── tasks/
│       │   └── main.yml      # Tasks for configuring Terminator
└── README.md                 # Project documentation
```

### Contributing
Contributions are welcome! If you have suggestions or improvements, please fork the repository and submit a pull request.

### License
This project is licensed under the MIT License. See the LICENSE file for more details.

---