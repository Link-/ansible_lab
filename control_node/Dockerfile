# Ansible Control Container
# will be used to manage the other nodes
FROM centos:latest

# Install Ansible and dependencies
RUN yum install -y epel-release
RUN yum install -y ansible
RUN yum -y install openssh-server openssh-clients

# Create the work directories
RUN mkdir -p /app/src
WORKDIR /app/src
COPY ./src /app/src

# Change the default sshd configuration
RUN cp -f /app/src/ssh_config/sshd_config /etc/ssh/sshd_config
# Create host keys
RUN ssh-keygen -A
# Move ssh_keys to the .ssh directory
RUN mkdir -p ~/.ssh && \
    mv /app/src/ssh_keys/id_rsa ~/.ssh/id_rsa && \
    chmod -R go= ~/.ssh && \
    chmod 600 ~/.ssh/id_rsa

# Expose SSH port
EXPOSE 22
# Entry point to keep the container continuously running
# since we don't have a single process that this container's
# supposed to maintain
ENTRYPOINT ["tail", "-f", "/dev/null"]