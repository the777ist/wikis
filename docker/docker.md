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

#### Creating containers using Dockerfile:
- **Dockerfile** contains config for container:
    - `FROM`: specifies base image
    - `RUN`: specifies commands to run to install additional programs, etc.
    - `CMD`: specifies container startup command

- Each step: 
    - takes image
    - creates intermediate containers 
    - performs operations
    - takes FS snapshot from container, generates an image, then intermediate container is removed
    - image used in subsequent steps

- Final image can then be used to create container using `docker run <image_name>`.
- Images are cached for use in future when identical steps are encountered.
