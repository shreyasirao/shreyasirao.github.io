---
title: Decker Notes
date: 2023-05-02 15:27:31
---
Unlike Hypervisors, Docker is not meant to virtualize and run different operating systems and kernels on the same hardware. The main purpose of Docker is to package and containerize applications and to ship them and to run them anywhere, anytime, as many times as you want.

Most organizations have their products containerized and available in a public Docker repository called **Docker Hub**.

### Docker Image
An image is a package or a template. It is used to create one or more Containers. 

### Containers
Containers are running instances of images that are isolated and have their own environments. Containers are completely isolated environments. They can have their own processes or services, their own network interfaces, their own mounts, just like virtual machines but they all share the same OS kernel.

### Matrix from hell
There maybe web server, database, messaging, orchestration-tools etc all needed in one project. Now all service's compatibility with underlying OS, libraries and dependencies compatibility with the OS could cause issues. This compatibility matrix issue is -> **matrix from hell**
New developers setup their environment, right OS, right versions. Run each component in a separate Container with its own dependencies and its own libraries, all on the same VM and the OS, but within separate environments or Containers. 

> OS= OS Kernel(interacts with hardware) + set of softwares

update apt package --> `sudo apt-get update`
install packages to allow apt to to use a repo over https --> `apt-get install` 

## DOCKER COMMANDS

1. `docker run <name of image>` - runs a container from an image
if image is not present will get it from docker hub and then reuse it
        `-d` - to run in detached mode or `bg` so you can do other things on cmd

2. `docker attach <name or id of container>` - to attach back to container after the detach mode
3. `docker logs <name or id of container>` - for container in detached mode to see it's logs


`-i` - is for interactive mode
`-t` - to attach to terminal for output prompts from the container
`-it` - attach to terminal and in interactive mode
By default docker container does not listen to stdin and is non interactive 

4. `-p 80:5000` - map port 80 on host to port 5000 on internal docker container
 Every docker container gets an ip assigned by default - but is an internal ip and only accessible from within the host
 so we need to map the port inside the docker container to free port on docker host

5. `-v <dir on host>:<dir on container>` - When you delete or remove a container all the data inside the container also vanishes. To persist data map a directory on host to a dir in container.
 
6. `-e <env_variable>=<value>` - pass environment variables when running the image

7. `--links <name of container>:<name of host current container is looking for>` - cmd line option to link 2 containers together


8. We can also run multiple instances of the application and map it to different ports on host
 
`docker run redis:4.0 `-> here 4.0 is the specific version we want for redis, by default it is latest. and `redis:4.0` is called a tag. Docker pulls an image of redis 4.0 version and runs that 

9. `docker pull <name of image>`- only pulls the image and not run the container

10. `docker ps` - lists all running containers

11. `docker inspect <name or id of container>` - to get all the details of a container in JSON format

12. `docker images` - lists all available images

13. `docker rm <name of container>` - to remove a stopped or exited container permanently

14. `docker rmi <name of image>`- can only delete an image only after all dependent containers are also deleted


> Containers are meant to run a specific task or process and once task is complete, container exits
you can instruct docker to run a process with docker run command

15. `docker exec`- command to run a task in the container


## CREATING YOUR OWN DOCKER IMAGES:
1. create Dockerfile with instructions to setup your application
format for Dockerfile-
`INSTRUCTION arguments
#####################################################################################
FROM Ubuntu - always starts with FROM bcz every image must be based of another image
RUN apt-get update - run a command on those base images update, install etc
COPY . /opt/source-code - copies file from host to image, so from current folder on local host to /opt/source-code folder in docker image
ENTRYPOINT <commands>- specify commands that will be run when image is run as a container
########################################################################################`

each line of instuction creates a new layer in docker image and 
each layer stores changes from previous layer only, so size keeps decreasing. Size can be checked with
docker history <name of image>

2. `docker build Dockerfile -t <account name/name for image>` - to create an image locally on our system
you can see the various steps involved and output from each task during the build
all layers built are cached, so you can restart docker build from any step, it automatically does that in case of failure or any new step is added. Only layers above the updated layer need to be rebuilt.

3. `docker push <name of image>` - make it available on public docker hub registry
by default it will be denied since it tries to push to the docker.io/library folder which is for official docker supported repos
We can push to repos under our own account. So we need to login into docker account. Tag the build to our account name first using `-t`

### commands, arguments and entrypoints -:

1. `CMD sleep 5 or
CMD["sleep","5"]` - defines the program to be run in the container
now this can be replaced by cmd line completely

2. `ENTRYPOINT ["sleep"]
CMD ["5"]` - 
Whatever is specified on cmd  line is appended to ENTRYPOINT instruction and if nothing is specified 5 is the default value


3. `Docker compose` -: docker-compose.yml
if we need to run multiple services the best way to do it is using docker compose
create a configuration file in YAML format 

4. `docker-compose up` - command to bring up the entire stack
if image is not available replace `image:<image name>` tag with `build:<folder where application code and Dockerfile to build image is stored>`. No need to use links in docker version 2 and above since it automatically creates a dedicated bridge network and attaches all containers to it

7. `depends_on` - to specify the order in which containers should run.

8. create our own networks using the `networks` tag:

`version:3
services:
	 redis:
		image: redis
		networks: 
			- net_2
	 db:
		image: postgres
		networks: 
			- net_1
networks: 
	net_1:
	net_2:`
	

9. 'Docker Registry': Central repository of all docker images
> image name convention- `<docker.io is default location from where images are pulled>/<user or account name>/<image or repository name>`


