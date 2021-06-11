# DOCKER

#### Image vs. Container:
- Image: A file containing all the deps and config required to run a program.  
- Container: Images can be used to create containers, which are *instances* of that image.
- Container is a running program with its own isolated set of hardware resources.  

#### Docker Client and Server:
- `docker-cli` (Docker Client) takes commands from user and runs them on `docker-daemon` (Docker Server) to create images, run containers, etc.

#### Namespacing and Control Groups:
- Segregating resources for each specific process. 
- Restricting the amount of resources that each specific process can use.

#### Containers:
- So a container is a process that has a certain amount of resources assigned to it.
- Container: Running process + Segment of resources assigned to it.
- Image contains a file system snapshot which is loaded during creation of container.

#### Docker commands:

- Check docker installation and version:

        $ docker version

- Create and start a container:

        $ docker run <image_name> <optional_command>

        # optional command, if specfied, is set as default command 
        # every time container is started, it will be executed 
        # if container aready has a default command, this will override it

        $ docker run -d <image_name>

        # -d starts the container in detached mode, i.e. frees up the prompt and container runs in background

- `docker run` = `docker create` + `docker start`

- Create a container: *Taking the file system snapshot from the image and loading it into container*

        $ docker create <image_name>

        # returns container_id

- Start a container: *Running the startup command specified in the image to start the container*

        $ docker start -a <container_id>

        # --attach or -a = shows verbose output/errors from container

- See all running containers:

        $ docker ps

- See all containers ever created on the machine:

        $ docker ps --all

- Remove all stopped containers:

        $ docker system prune

- See container logs:

        $ docker logs <container_id>

- Stop/kill container: *Stop: issues SIGTERM (terminate) message allowing process 10 sec to clean up before stopping* whereas *Kill: issues SIGKILL (kill) message causing process to stop immediately*

        $ docker stop <container_id>
        $ docker kill <container_id>

- Executing commands inside running containers:

        $ docker exec -it <container_id> <command>

        # -it = allows to pass input to the container

- Getting shell prompt to container for general purpose UNIX commands:

        $ docker exec -it <container_id> sh
        # or
        $ docker exec -it <container_id> bash

        # ctrl+c or ctrl+d or type 'exit' to exit out of container

#### Dockerfile:
- **Dockerfile** contains config for container, builds an image that can be run as a container:
    - `FROM`: specifies base image
    - `RUN`: specifies commands to run to install additional programs, etc.
    - `CMD`: specifies container startup command

- Each step: 
    - takes image
    - creates intermediate containers 
    - performs operations
    - takes FS snapshot from container, generates an image, then intermediate container is removed
    - image used in subsequent steps

- Final image can then be used to create container using `docker run <image_name>`
- Images are cached for use in future when identical steps are encountered.

- Build an image and run container using `Dockerfile`:

        $ docker build <path_to_Dockerfile>
        # or
        $ docker build <url>

        # returns container_id

        $ docker run <container_id>

- Tagging a docker image while building from `Dockerfile`:

        $ docker build -t <user_docker_id>/<repo_name or project_name>:<version> <path_to_Dockerfile>

        # -t = tags the image

#### Dockerizing a simple Nodejs microservice:

- Dockerfile:

        FROM node:<tag>

        ENV <env_var_name> <value>

        WORKDIR <path_to_dir>
        
        COPY ./package*.json ./
        # syntax: COPY <path_on_machine> <path_in_container>

        RUN npm install

        COPY ./ ./

        CMD [ "npm", "start" ]

- `cd` into dir containing Dockerfile

- Build image from Dockerfile:

        $ docker build -t <user_docker_id>/<repo_name or project_name>:<version> .
                        
- Start container with port mapping (unless port forwarding is set up explicitly):

        $ docker run -p <external_port>:<internal_port> <image_name>

#### Docker compose for multiple local containers:

- Docker Compose can be used to start up multiple containers at the same time.

- Say we need separate containers for a nodejs app and redis in same machine.

- `docker-compose.yml`:

        version: '<version_of_docker_compose>'

        services:            
            # start a container running redis-server using the base image 'redis'
            redis-server:
                image: 'redis'

            # start a container running the nodejs app
            node-app:
                # restart policy
                restart: on-failure
                
                # build image using Dockerfile in the current dir
                build: .
                
                # port mapping
                ports:
                    - '<external_port>:<internal_port>'

- In nodejs app, configure host of redis as `redis-server` while configuring the database driver, same for anything else, like say postgres i.e. in place of host, write the container name.

- Start containers:

        $ docker-compose up -d

        # -d starts containers in detached mode 
        # this is same as 
            $ docker run <image_name>

- Build from images and start containers:
    
        $ docker-compose up --build 

        # this is same as 
            $ docker build . 
            $ docker run <image_name

- Stop containers:
    
        $ docker-compose down

- See status of running containers:
    
        $ docker-compose ps

        # must be run from same dir containing the docker-compose.yml file
        # this is same as 
            $ docker ps

- `restart policy` in `docker-compose.yml` can be any of the following:
    - `"no"`: never attempt to restart container if it stops/crashes
    - `always`: always attempt to restart container if it stops
    - `on-failure`: only attempt to restart container if it stops with an error code (other than exit code 0)
    - `unless-stopped`: always attempt to restart container, unless forcibly stopped  

