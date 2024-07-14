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
