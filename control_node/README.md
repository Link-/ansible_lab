# Control Node

## Setup

```
# Buikd
docker build -f ./Dockerfile -t ansible/control_node:v1 .

# Run
docker run -d --name control_node ansible/control_node:v1

# Shell
docker exec -it control_node /bin/sh
```