fix "permission denied socker error":
```bash
sudo groupadd docker
sudo usermod -aG docker username
getent group docker # -> docker:x:999:yyev89
sudo systemctl restart docker
```

test after install:
```bash
docker run hello-world
```

run postgres container:
```bash
docker run --env POSTGRES_PASSWORD=foobar --publish 5432:5432 postgres:15.1-alpine
```

run ubuntu with interactive shell and remove it after it exits:
```bash
docker run --interactive --tty --rm ubuntu:22.04
# Same, but NOT deleting it:
docker run --interactive --tty --name my-ubuntu-container ubuntu:22.04
```

list all running containers:
```bash
docker ps
# List all containers:
docker ps -a
```

start container:
```bash
docker start my-ubuntu-container
```

attach to the running container:
```bash
docker attach my-ubuntu-container
```

build custom image with ubuntu as base and ping installed:
```bash
docker build --tag my-ubuntu-image -<<EOF
FROM ubuntu:22.04
RUN apt update && apt install iputils-ping --yes
EOF
```

create a named volume:
```bash
docker volume create my-volume
```

create a container and mount the volume into the container filesystem:
```bash
docker run -it --rm --mount source=my-volume,destination=/my-data/ ubuntu:22.04
# Shorter version:
docker run -it --rm -v my-volume:/my-data ubuntu:22.04
```

create a continer and mount a directory from host filesystem into the container (bind mounts variant):
```bash
docker run -it --rm -v ${PWD}/my-data:/my-data ubuntu:22.04
```

build an image with tag in current dir:
```bash
docker build -t api-golang:1 .
```

### Running and docker-compose

run container in detatched mode:
```bash
docker run -d ubuntu sleep 99
```

overwrite entrypoint from Dockerfile:
```bash
docker run --entrypoint echo ubuntu hello
```

set environment variables at runtime:
```bash
docker run --env MY_ENV=hello ubuntu printenv
```

change PID from docker-init process:
```bash
docker run --init ubuntu ps
```

run with interactive shell (tty):
```bash
docker run -it ubuntu
```

specify name for the container:
```bash
docker run -d --name my-container ubuntu sleep 99
```

attach container to a specific network:
```bash
docker network ls
docker network create my-network
docker run -d --network|--net my-network ubuntu sleep 99
```

run with different architecture:
```bash
docker run --platform linux/arm64/v8 ubuntu dpkg --print-architecture
```

set restarting options (always, unless-stopped, never):
```bash
docker run --restart unless-stopped ubuntu
```

build, run, stop and remove containers with docker-compose.yml:
```bash
# Build:
docker compose build
# Build and run:
docker compose up --build
# Run attached:
docker compose up
# Run not attached:
docker compose up -d
# Stop:
docker compose stop
# Remove everything (+volumes, +networks etc):
docker compose down
```

delete all containers:
```bash
docker rm $(docker ps -aq) -f
```

delete all images:
```bash
docker rmi $(docker images -q)
```

`Ctrl+PQ`  detach from running container without killing it

list all builders:
```bash
docker buildx ls
# Choose one to use:
docker buildx use <name>
```

build a container for multiple architectures:
```bash
docker buildx build --platform=linux/amd64,linux/arm64 -t yyarynich/ddd-book:ch8.1 --push .
```