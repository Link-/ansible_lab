# Ansible Lab
> A simple setup to help you learn Ansible

This project is a combination of 2 containers running CentOS images that can be used to execute Ansible modules and playbooks. The containers come pre-loaded with all the needed packages and dependencies and the configuration is done so that you can access the `managed nodes` from the `control node`. More on this below.

## Project Structure

```
.
├── README.md
├── control_node
│   ├── Dockerfile
│   ├── README.md
│   └── src
│       ├── ansible.cfg
│       ├── inventory
│       ├── playbooks
│       │   └── demo.yaml
│       ├── ssh_config
│       │   └── sshd_config
│       └── ssh_keys
│           ├── id_rsa
│           └── id_rsa.pub
├── docker-compose.yml
└── managed_node
    ├── Dockerfile
    ├── README.md
    └── src
        ├── ssh_config
        │   └── sshd_config
        └── ssh_keys
            └── id_rsa.pub

9 directories, 14 files
```

## Setup

```bash
docker-compose up -d --build

#> Output
Creating network "ansible_lab_default" with the default driver
Building control_node
...
...

Successfully built 1c2b7a4936bf
Successfully tagged ansible/managed_node:v1
Creating control_node ... done
Creating second_managed_node ... done
Creating first_managed_node  ... done
```

## Ansible

### Sample Commands

```
# Go into the control node
docker exec -ti control_node /bin/sh

# Execute Ansible commands
ansible all -i inventory --list-hosts

# Execute a shell command on all managed nodes
ansible all -m command -a "echo hello!"

# Create user lina on managed node
ansible first_managed_node -m user -a "name=linda shell=/bin/bash"

# Fetch list of users on a managed node
# the shell module allows you to use pipes
ansible first_managed_node -m shell -a "cat /etc/passwd"
```

### Playbooks

```
```