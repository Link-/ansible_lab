# Managed Nodes

## Setup

```
# Build
docker build -f ./Dockerfile -t ansible/managed_node:v1 .

# Run
docker run -d --name managed_node_primary ansible/managed_node:v1
docker run -d --name managed_node_secondary ansible/managed_node:v1

# Shell
docker exec -it <container name> /bin/sh
```