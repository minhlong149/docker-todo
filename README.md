# Containers

Learn how to package code into standard units of software by utilize containers to create immutable execution environments, from the [Helsinki-based Services Foundation team](https://fullstackopen.com/en/part12) at Unity, in collaboration with Jami Kousa.

## Table of contents

- [Containers](#containers)
  - [Table of contents](#table-of-contents)
  - [Introduction](#introduction)
    - [Containers \& Images](#containers--images)
    - [Ubuntu images](#ubuntu-images)
  - [Building and configuring environments](#building-and-configuring-environments)
    - [Dockerfile](#dockerfile)
    - [Dockerfile best practices](#dockerfile-best-practices)
    - [Using Docker compose](#using-docker-compose)
  - [Basics of Orchestration](#basics-of-orchestration)
  - [Resources](#resources)

## Introduction

- Containers is a modern tool utilized in the latter parts of the software development lifecycle, by **encapsulate your application into a single package**, which can be run on any machine that has a container runtime installed.

- Containers are OS-level virtualization. Compare to virtual machines (VMs), which is used to run multiple operating systems on a single physical machine, a container **using the host operating system**. This makes containers lightweight and fast.

- Containers are beneficial when developing new applications that **needs to run on the same machine as a legacy application**, for they are isolated from each other and do not interfere with each other. Or when you need to **move the application from your local machine** to a server. It is not uncommon that the application just does not run on the server despite "it works on my machine".

### Containers & Images

- A **container** is a runtime instance of an **image**. **Images** include all of the code, dependencies and instructions on how to run the application, while **containers** package software into standardized units. _Containers only exist during runtime._

> You **_CANNOT_** edit an image after you have created one. However, you can use existing images to create a new image by adding new layers on top of the existing ones.

- [Docker](https://www.docker.com/), a set of products that is used to **manage** images and containers, is the most popular containerization technology use today. [Docker Compose](https://docs.docker.com/compose/) is a tool that allows one to **orchestrate** (control) **multiple containers** at the same time.

- To create a container from an image, run `docker run <IMAGE-NAME>`. If the image does not exist locally, Docker will download it from [Docker Hub](https://hub.docker.com/). You can also specify the version of the image by adding a colon and the version number after the image name.

> Each image has a unique _digest_ based on the layers from which the image is built. In practice, each step or command that was used in building the image creates a unique layer. The digest is used by Docker to identify that an image is the same. This is done when you try to pull the same image again. _If the image is the same, Docker won't download it again._

### Ubuntu images

- Run an Ubuntu container with the command `docker run -it ubuntu bash`. The `-it` flag is used to make the container interactive, so that you can use the terminal inside the container. After the options, we defined that image to run is `ubuntu`. Then we have the command `bash` to be executed inside the container when we start it.

- To list all the containers that _have been run_, run `docker ps -a`. You can start a stopped container with `docker start <CONTAINER-ID-OR-CONTAINER-NAME>`. To stop a running container, run `docker stop <CONTAINER-ID-OR-CONTAINER-NAME>` or `docker kill <CONTAINER-ID-OR-CONTAINER-NAME>`.

> The difference between `docker stop` and `docker kill` is that `docker stop` sends a SIGTERM signal to the process running inside the container, while `docker kill` sends a SIGKILL signal. The SIGTERM signal tells the process to stop, while the SIGKILL signal tells the kernel to kill the process immediately. The process can't ignore the SIGKILL signal, but it can ignore the SIGTERM signal. _Check out [this article](https://www.baeldung.com/ops/docker-stop-vs-kill) for more information._

## Building and configuring environments

### Dockerfile

- Dockerfile is a text file that contains all of the instructions for creating an image.

- The first line of a Dockerfile is the `FROM` instruction, which defines the **base image** for the image that is being built. The base image is the image that is used as the starting point for the new image. The base image is usually an image that is provided by the community, such as `ubuntu` or `node`. The base image is the only required instruction in a Dockerfile.

- The `WORKDIR` instruction is used to **set the working directory** for the commands that are run inside the container, ensure we don't interfere with the contents of the image.

- The `COPY` instruction is used to **copy files** from the host machine to the container.

- The `RUN` instruction is used to run commands inside the container. The `RUN` instruction is used to **install dependencies** and other tools that are needed to run the application.

- The `CMD` instruction is used to define the command that is run when the container is started. The `CMD` instruction is used to **start the application**.

- Use the command `docker build .` to build an image based on the Dockerfile. The period at the end of the command is the path to the directory where the Dockerfile is located. After the build is finished, you can run it with `docker run <IMAGE-ID-OR-IMAGE-NAME>`. You can also run the image with `docker run <IMAGE-ID-OR-IMAGE-NAME> <COMMAND>`, where `<COMMAND>` is the command that you want to run inside the container.
  - You can specify the name of the image with the `-t <IMAGE-NAME>` flag.
  - You can also inform Docker that a port from the host machine should be opened and directed to a port in the container, with the `-p` flag. The format for it is `-p <HOST-PORT>:<CONTAINER-PORT>`. It's usually the same port number for both.

### Dockerfile best practices

- Use explicit and deterministic Docker base image tags, for example, `FROM node:18`. This ensures that the same version of the base image is used every time the image is built. Also consider using a smaller base image, such as `slim`. `alpine` image tags are even smaller, but they are not officially supported by Node.js.

- Run `npm ci` to install dependencies during the build process inside the container, instead of `npm install` prior to building. Even better, add `--only=production` flag o not waste time installing development dependencies.

> `npm ci` is used to install the dependencies from the `package-lock.json` file, while `npm install` is used to install the dependencies from the `package.json` file. `npm ci` is faster and more reliable than `npm install`.

| `npm install`                                                                                       | `npm ci`                                                    |
| --------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| update the `package-lock.json`                                                                      | follow the `package-lock.json` and does not alter any files |
| may install a different version of a dependency if you have ^ or ~ in the version of the dependency | delete the node_modules folder before installing anything   |

- Keeping unnecessary files out of your Node.js Docker images will reduce the size of the image. Use `.dockerignore` to prevent unwanted files from being copied to your image, such as `node_modules`.

- Some frameworks and libraries may optimize the performance if the `NODE_ENV` environment variable is set to `production`. You can set the `NODE_ENV` environment variable with the `ENV` instruction in the Dockerfile.

- Use multi-stage builds to reduce the size of the image. Multi-stage builds allow you to use multiple `FROM` instructions in a Dockerfile. Each `FROM` instruction starts a new stage of the build. The final image will only contain the files from the last stage. This is useful when you need to install dependencies during the build process, but you don’t need them in the final image.

- Don’t run containers as root. Use the `USER` instruction to run the container as a non-root user.

### Using Docker compose

## Basics of Orchestration

## Resources

- Fireship - [Learn Docker in 7 Easy Steps](https://youtu.be/gAkwW2tuIqE)
- Jeff Delaney - [Docker Basics Tutorial with Node.js](https://fireship.io/lessons/docker-basics-tutorial-nodejs/)
- Tôi Đi Code Dạo - [Tự học Docker siêu tốc trong 10 phút](https://youtu.be/1k8pox8mkxc)
