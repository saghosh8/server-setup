# Ansible Automation Project

This Ansible automation project is designed to streamline server setup, package installation, network configuration, and securing access across development (Dev), staging (Stage), and production (Prod) environments. The project leverages Ansible roles to modularize tasks, ensuring maintainability and reusability of the automation scripts.

## Directory Structure

The project is organized as follows:

```
ansible-project/
├── ansible.cfg
├── inventories/
│   ├── dev/
│   │   └── hosts
│   ├── stage/
│   │   └── hosts
│   └── prod/
│       └── hosts
├── playbooks/
│   └── site.yml
├── roles/
│   ├── server_setup/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   └── templates/
│   │       └── hostname.j2
│   ├── install_packages/
│   │   └── tasks/
│   │       └── main.yml
│   ├── configure_network/
│   │   └── tasks/
│   │       └── main.yml
│   └── secure_access/
│       └── tasks/
│           └── main.yml
└── vars/
    └── main.yml
```

### Configuration Files

- **`ansible.cfg`**: Configures Ansible to use the specified inventory directory and roles path.
- **`inventories/`**: Contains environment-specific inventory files (`dev`, `stage`, `prod`).
- **`playbooks/site.yml`**: The main playbook that applies roles to all hosts in the inventory.
- **`roles/`**: Contains roles that modularize tasks for server setup, package installation, network configuration, and securing access.
- **`vars/main.yml`**: Defines common variables used across the roles.

## Roles

### 1. Server Setup (`server_setup`)

**Purpose**: Sets up basic server configuration, including hostname, user accounts, and groups.

**Tasks**:
- Sets the hostname based on the environment.
- Ensures the `sudo` group and `deploy` user are present on the system.
- Applies the new hostname configuration.

### 2. Install Packages (`install_packages`)

**Purpose**: Installs common packages required on the servers.

**Tasks**:
- Installs a list of common packages (e.g., `git`, `vim`, `curl`) using the appropriate package manager for the target system.

### 3. Configure Network (`configure_network`)

**Purpose**: Configures network interfaces on the servers.

**Tasks**:
- Configures the network interfaces using `nmcli`.
- Applies the network configuration to ensure the settings are active.

### 4. Secure Access (`secure_access`)

**Purpose**: Secures server access by configuring firewall rules and SSH access.

**Tasks**:
- Configures firewall rules to allow SSH access using `firewalld`.
- Adds SSH authorized keys for the `deploy` user to allow secure access.

## Variables

The `vars/main.yml` file contains variables that define common packages to install and SSH authorized keys. These variables ensure consistency across different environments.

```yaml
common_packages:
  - git
  - vim
  - curl

dev_hostname: "dev-{{ inventory_hostname }}"
stage_hostname: "stage-{{ inventory_hostname }}"
prod_hostname: "prod-{{ inventory_hostname }}"

authorized_keys:
  - "ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAr..."
```

## Running the Playbook

To apply the configurations to a specific environment, navigate to the `ansible-project` directory and run the playbook with the appropriate inventory file:

```bash
ansible-playbook -i inventories/dev/hosts playbooks/site.yml
ansible-playbook -i inventories/stage/hosts playbooks/site.yml
ansible-playbook -i inventories/prod/hosts playbooks/site.yml
```

This command will execute the `site.yml` playbook using the inventory file for the specified environment, applying the roles and tasks defined in the project.

## Conclusion

This Ansible automation project provides a structured approach to managing server configurations across different environments. By leveraging roles and variables, the project ensures modularity, reusability, and consistency, making it easier to manage infrastructure as code.
