# Ansible Managed Container
# will be used to manage the other nodes
FROM centos:latest

# Define arguments
ARG HOST_NAME

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
    mv /app/src/ssh_keys/id_rsa.pub ~/.ssh/id_rsa.pub && \
    touch ~/.ssh/authorized_keys && \
    chmod -R go= ~/.ssh && \
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

# Expose SSH port
EXPOSE 22
# Run SSH daemon
CMD ["/usr/sbin/sshd", "-D"]