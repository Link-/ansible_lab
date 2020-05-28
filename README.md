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

### Nodes

#### Control Node
This is mainly used to run modules and playbooks that are applied on the `managed nodes`. The `Dockerfile` is setup to copy all the files in `src` to `/app/src` inside the container. This control node also has the `private key: id_rsa` that is used to SSH into the managed nodes

#### Managed Nodes
There are 2 managed nodes containers that are spun up with `docker-compose` the: `first_managed_node` and `second_managed_node`. Both of these nodes have the `public key: id_rsa.pub` configured as an `authorized key` to allow the control node to SSH and execute ansible modules and playbooks inside these containers.

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

#### Run commands from host
You don't have to go into the control_node container to execute commands on the managed nodes.

```bash
# Execute a command on all managed nodes
docker exec control_node ansible all -m command -a "echo hello!"

#> Output
first_managed_node | CHANGED | rc=0 >>
hello!
second_managed_node | CHANGED | rc=0 >>
hello!

# Fetch list of users on a managed node
# the shell module allows you to use pipes
docker exec control_node ansible first_managed_node -m shell -a "cat /etc/passwd"

#> Output
first_managed_node | CHANGED | rc=0 >>
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
systemd-resolve:x:193:193:systemd Resolver:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
```

#### Run commands from the control_node
You can go into the control_node container to execute commands on the managed nodes, or check the following sections for an alternative.

```bash
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

### Playbooks

You can run playbooks that are stored in the directory `src/playbooks`. You can add your own playbooks to the directory just make sure to rebuild the containers so that the files are copied to the control node.

#### src/playbooks/demo.yaml
```bash
# Run the demo playbook to test the setup
docker exec control_node ansible-playbook playbooks/demo.yaml

#> Output

PLAY [test_debug] **************************************************************

TASK [Gathering Facts] *********************************************************
ok: [first_managed_node]
ok: [second_managed_node]

TASK [first_task] **************************************************************
ok: [first_managed_node] => {
    "msg": "All works fine and dandy!"
}
ok: [second_managed_node] => {
    "msg": "All works fine and dandy!"
}

TASK [Fetch IP addresses] ******************************************************
ok: [first_managed_node] => {
    "msg": "This host uses the IP: 172.19.0.4"
}
ok: [second_managed_node] => {
    "msg": "This host uses the IP: 172.19.0.3"
}

PLAY RECAP *********************************************************************
first_managed_node         : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
second_managed_node        : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

---

## Caveats
- The above setup has only been tested on macOS

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