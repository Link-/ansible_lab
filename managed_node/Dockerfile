# Ansible Control Container
# will be used to manage the other nodes
FROM centos:latest

# Install Ansible and dependencies
RUN yum install -y epel-release
RUN yum install -y ansible

# Entry point to keep the container continuously running
# since we don't have a single process that this container's
# supposed to maintain
ENTRYPOINT ["tail", "-f", "/dev/null"]