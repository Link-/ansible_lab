# Ansible Lab

## Setup

```bash
docker-compose up -d --build

#> Output
Creating network "ansible_lab_default" with the default driver
Building control_node
...

Successfully built 1c2b7a4936bf
Successfully tagged ansible/managed_node:v1
Creating control_node ... done
Creating second_managed_node ... done
Creating first_managed_node  ... done
```

## Ansible

### Commands

```
ansible all -i inventory --list-hosts
```