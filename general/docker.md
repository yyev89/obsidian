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

**General Principles for Dockerfiles (by Cid Palas):** 
_Make it work, make it secure, make it fast_

-  Pin specific versions 🔒👁️  
-  Base images (either major+minor OR SHA256 hash)  🚗👁️
-  System Dependencies  🔒👁️ 
-  Application Dependencies  🔒👁️
-  Use small + secure base images  🔒
-  Protect the layer cache  🚗
-  Order commands by frequency of change  🚗
-  COPY dependency requirements file → install deps → copy remaining source code  🚗 
-  Use cache mounts  🚗
-  Use COPY --link  🚗
-  Combine steps that are always linked (use heredocs to improve tidiness) 🚗👁️
-  Be explicit  🔒👁️
-  Set working directory with WORKDIR  🔒👁️
-  Indicate standard port with EXPOSE  👁️
-  Set default environment variables with ENV  🔒👁️
-  Avoid unnecessary files  🔒🚗
-  Use .dockerignore  🔒🚗
-  COPY specific files  🔒🚗
-  Use non-root USER  🔒
-  Install only production dependencies  🔒
-  Avoid leaking sensitive information  🔒
-  Leverage multi-stage builds ​🔒🚗

Agenda:
- 🔒Security
- 🚗 Built speed
- 👁️ Clarity

### Registries

build simple blank image:
```bash
echo "FROM scratch" > Dockerfile
docker build --tag my-scratch-image .
rm Dockerfile
```

login to dockerhub:
```bash
docker login
```

retag image associated with your repo:
```bash
docker tag my-scratch-image yyarynich/my-scratch-image:abc-123
# latest by default:
docker tag my-scratch-image yyarynich/my-scratch-image
```

push to dockerhub:
```bash
docker push yyarynich/my-scratch-image:abc-123
# latest by default:
docker push yyarynich/my-scratch-image
```
