# DOCKER COMMANDS:

Check docker installation and version:

    $ docker version

Create and start a container:

    $ docker run <image_name> <optional_command>

    # optional command, if specfied, is set as default command 
    # every time container is started, it will be executed 

`docker run` = `docker create` + `docker start`

Create a container:  
*Taking the file system snapshot from the image and loading it into container*

    $ docker create <image_name>

    # returns container_id

Start a container:  
*Running the startup command specified in the image to start the container*

    $ docker start -a <container_id>

    # --attach or -a = shows verbose output/errors from container

See all running containers:

    $ docker ps

See all containers ever created on the machine:

    $ docker ps --all

Remove all stopped containers:

    $ docker system prune

See container logs:

    $ docker logs <container_id>

Stop/kill container:  
*Stop: issues SIGTERM (terminate) message allowing process 10 sec to clean up before stopping*  
*Kill: issues SIGKILL (kill) message causing process to stop immediately*

    $ docker stop <container_id>
    $ docker kill <container_id>

Executing commands inside running containers:

    $ docker exec -it <container_id> <command>

    # -it = allows to pass input to the container

Getting shell prompt to container for general purpose UNIX commands:

    $ docker exec -it <container_id> sh
    # or
    $ docker exec -it <container_id> bash

    # ctrl+c or ctrl+d or type 'exit' to exit out of container

Build an image and run container using `Dockerfile`:

    $ docker build <path_to_Dockerfile>
    # or
    $ docker build <url>

    # returns container_id

    $ docker run <container_id>

Tagging a docker image while building from `Dockerfile`:

    $ docker build -t <user_docker_id>/<repo_name or project_name>:<version> <path_to_Dockerfile>

    # -t = tags the image



