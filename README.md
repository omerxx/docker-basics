# Developing on and with Docker

![docker](https://docker-curriculum.com/images/logo.png)

### What is Docker?
The most common containerization platform.
A container is a small package the contains the entire requirements to run a program.

Container vs VM: Docker is our hypervisor, and there's no need for a guest OS and its resources.

It's lighter, faster, uses smart mechanisms for caching and building new containers in "layers".
Layers can be shared among containers, and are used to save space and time when interacting with containers, whether by build / ship / pull / push / run etc..


### How do I get docker to my machine?
Used to be a package of docker-machine, and some libs, but life is simpler now:
If you're using mac download [docker for mac](https://www.docker.com/docker-mac).


### CheatSheet
```bash
# Pulling oficials from store.docker.com
$ docker pull nginx

# Pulling a public image
$ docker pull omerxx/go-health

# Listing
$ docker images
$ docker images | grep health

# Let's run it
$ docker run omerxx/go-health
# Expose ports
$ docker run -p 8000:8000 omerxx/go-health

# What is running?
$ docker ps
$ docker ps -a

# Let's see what's inside
$ docker exec -it <container-id> bash	# *on alpine we'll have to use /bin/sh

# Run a container and get a shell session
$ docker run -it <container-id> bash	# *on alpine we'll have to use /bin/sh

# Some stats - CPU, MEM, etc
$ docekr stats

# Save the current state with commit
$ docker commit <container-id> my-container

# Kill it!
$ docker kill <container-id>
# Kill everything?
$ docker kill $(docker ps -q)

# Delete an image
$ docker rmi omerxx/go-health
$ docker images | grep health 		# *Verifying
```


### Dockerfiles
Dockerfiles are our way of creating a container with a few simple declarative lines:
```dockerfile
# Setting our base image
FROM ubuntu / alpine / spotim/conversation / scratch

# Environment
ENV KEY=VALUE \
    KEY=VALUE

# Run stuff
RUN echo "hello"

# Chain commands to keep them cached under the same layer:
RUN yum install -y curl && curl https://spot.im

# Bring your files
ADD . /app 		# ADD allows <src> to be a URL, and will be unpacked if an archive
# OR
# COPY . /app 		

CMD ["npm", "start"] 	# CMD can be overriden when running the container, which cannot be done with ENTRYPOINT
```

### Every project should start from a docker-compose
Docker compose allows us to run /+ build clusters or single container groups with ease
```bash
# Running the compose file
$ docker-compose up

# Run and build containes!
$ docker-compose up --build

# Running only a specific component from the compose
$ docker-compose up redis

# Running detached (background mode)
$ docker-compose up -d

# Destroy
$ docker-compose down
```

### Developing workspace
```bash
# If I'd like to work on an already built image 
my-service:
  image: node:7.2.1
  volumes:
    - ./:/app
  command: ["tail", "-f", "/dev/null"]
```
Or:

```bash
# If I'd like to build my own
my-service:
  build: .
  volumes:
    - ./:/app
  command: ["tail", "-f", "/dev/null"]
```

### Working with ECR
```bash
# Add to your ~/.bashrc
alias ecr='aws ecr get-login --no-include-email --region us-east-1'

# Pull
docker pull 017894670386.dkr.ecr.us-east-1.amazonaws.com/<REPO>:<TAG>
```

### Productivity Tips
Add to your ~/.bashrc or ~/.zshrc etc:
```bash
# Docker
alias dco="docker-compose"
alias de="docker exec"
alias dr="docker run"
alias dsh="docker exec -it `$(docker ps -q)` bash"
alias dshalpine="docker exec -it $(docker ps -q) /bin/sh"
alias dps="docker ps"
alias dpa="docker ps -a"
alias di="docker images"
alias dl="docker ps -l -q"
```

