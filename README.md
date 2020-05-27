# Ansible Lab

## Setup

```bash
docker-compose up -d --build

#> Output
Creating network "ansible_lab_default" with the default driver
Building control_node
Step 1/4 : FROM centos:latest
 ---> 470671670cac
Step 2/4 : RUN yum install -y epel-release
 ---> Using cache
 ---> a1f9267e3213
Step 3/4 : RUN yum install -y ansible
 ---> Using cache
 ---> 1e61c9506607
Step 4/4 : ENTRYPOINT ["tail", "-f", "/dev/null"]
 ---> Using cache
 ---> 1c2b7a4936bf

Successfully built 1c2b7a4936bf
Successfully tagged ansible/control_node:v1
Building first_managed_node
Step 1/4 : FROM centos:latest
 ---> 470671670cac
Step 2/4 : RUN yum install -y epel-release
 ---> Using cache
 ---> a1f9267e3213
Step 3/4 : RUN yum install -y ansible
 ---> Using cache
 ---> 1e61c9506607
Step 4/4 : ENTRYPOINT ["tail", "-f", "/dev/null"]
 ---> Using cache
 ---> 1c2b7a4936bf

Successfully built 1c2b7a4936bf
Successfully tagged ansible/managed_node:v1
Building second_managed_node
Step 1/4 : FROM centos:latest
 ---> 470671670cac
Step 2/4 : RUN yum install -y epel-release
 ---> Using cache
 ---> a1f9267e3213
Step 3/4 : RUN yum install -y ansible
 ---> Using cache
 ---> 1e61c9506607
Step 4/4 : ENTRYPOINT ["tail", "-f", "/dev/null"]
 ---> Using cache
 ---> 1c2b7a4936bf

Successfully built 1c2b7a4936bf
Successfully tagged ansible/managed_node:v1
Creating control_node ... done
Creating second_managed_node ... done
Creating first_managed_node  ... done
```