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

#### Run commands from the control_node
You can go into the control_node container to execute commands on the managed nodes, or check the following sections for an alternative.
```
# Go into the control node
docker exec -ti control_node /bin/sh

# Execute Ansible commands
ansible all -i inventory --list-hosts

# Execute a command on all managed nodes
ansible all -m command -a "echo hello!"

# Create user lina on managed node
ansible first_managed_node -m user -a "name=linda shell=/bin/bash"

# Fetch list of users on a managed node
# the shell module allows you to use pipes
ansible first_managed_node -m shell -a "cat /etc/passwd"
```

#### Run commands from host
You don't have to go into the control_node container to execute commands on the managed nodes.

```
# Execute a command on all managed nodes
docker exec control_node ansible all -m command -a "echo hello!"

# Fetch list of users on a managed node
# the shell module allows you to use pipes
docker exec control_node ansible first_managed_node -m shell -a "cat /etc/passwd"
```

### Playbooks

#### Run commands from host
You can run playbooks that are stored in the directory `src/playbooks`. You can add your own playbooks to the directory just make sure to rebuild the containers so that the files are copied to the control node.

```
# Run the demo playbook to test the setup
docker exec control_node ansible-playbook playbooks/demo.yaml
```

---

## Caveats
- In all the examples above I've been using the fish shell which varies slightly from bash
- The above setup has only been tested on macOS
- Multiple configurations can be automated and some are hardcoded like the ECR URL in the deployments and gulp files

--- 

## Release History

* 0.1.0
    * Work in progress

---

## Meta

Copyright [2020] [Bassem Dghaidi](https://github.com/Link-)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

See ``LICENSE`` for more information.