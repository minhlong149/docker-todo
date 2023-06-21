# Containers

Learn how to package code into standard units of software by utilize containers to create immutable execution environments, from the [Helsinki-based Services Foundation team](https://fullstackopen.com/en/part12) at Unity, in collaboration with Jami Kousa.

## Table of contents

- [Containers](#containers)
  - [Table of contents](#table-of-contents)
  - [Introduction](#introduction)
    - [Containers \& Images](#containers--images)
    - [Ubuntu images](#ubuntu-images)
  - [Building and configuring environments](#building-and-configuring-environments)
  - [Basics of Orchestration](#basics-of-orchestration)
  - [Resources](#resources)

## Introduction

- Containers is a modern tool utilized in the latter parts of the software development lifecycle, by **encapsulate your application into a single package**, which can be run on any machine that has a container runtime installed.

- Containers are OS-level virtualization. Compare to virtual machines (VMs), which is used to run multiple operating systems on a single physical machine, a container **using the host operating system**. This makes containers lightweight and fast.

- Containers are beneficial when developing new applications that **needs to run on the same machine as a legacy application**, for they are isolated from each other and do not interfere with each other. Or when you need to **move the application from your local machine** to a server. It is not uncommon that the application just does not run on the server despite "it works on my machine".

### Containers & Images

- A **container** is a runtime instance of an **image**. **Images** include all of the code, dependencies and instructions on how to run the application, while **containers** package software into standardized units. *Containers only exist during runtime.*

> You ***CANNOT*** edit an image after you have created one. However, you can use existing images to create a new image by adding new layers on top of the existing ones.

- [Docker](https://www.docker.com/), a set of products that is used to **manage** images and containers, is the most popular containerization technology use today. [Docker Compose](https://docs.docker.com/compose/) is a tool that allows one to **orchestrate** (control) **multiple containers** at the same time.

- To create a container from an image, run `docker run <IMAGE-NAME>`. If the image does not exist locally, Docker will download it from [Docker Hub](https://hub.docker.com/). You can also specify the version of the image by adding a colon and the version number after the image name.

> Each image has a unique *digest* based on the layers from which the image is built.  In practice, each step or command that was used in building the image creates a unique layer. The digest is used by Docker to identify that an image is the same. This is done when you try to pull the same image again. *If the image is the same, Docker won't download it again.*

### Ubuntu images

- Run an Ubuntu container with the command `docker run -it ubuntu`. The `-it` flag is used to make the container interactive, so that you can use the terminal inside the container. After the options, we defined that image to run is `ubuntu`. Then we have the command `bash` to be executed inside the container when we start it.

- To list all the containers that *have been run*, run `docker ps -a`. You can start a stopped container with `docker start <CONTAINER-ID-OR-CONTAINER-NAME>`. To stop a running container, run `docker stop <CONTAINER-ID-OR-CONTAINER-NAME>` or `docker kill <CONTAINER-ID-OR-CONTAINER-NAME>`.

> The difference between `docker stop` and `docker kill` is that `docker stop` sends a SIGTERM signal to the process running inside the container, while `docker kill` sends a SIGKILL signal. The SIGTERM signal tells the process to stop, while the SIGKILL signal tells the kernel to kill the process immediately. The process can't ignore the SIGKILL signal, but it can ignore the SIGTERM signal. *Check out [this article](https://www.baeldung.com/ops/docker-stop-vs-kill) for more information.*

## Building and configuring environments

## Basics of Orchestration

## Resources

- Fireship - [Learn Docker in 7 Easy Steps](https://youtu.be/gAkwW2tuIqE)
- Jeff Delaney - [Docker Basics Tutorial with Node.js](https://fireship.io/lessons/docker-basics-tutorial-nodejs/)
- Tôi Đi Code Dạo - [Tự học Docker siêu tốc trong 10 phút](https://youtu.be/1k8pox8mkxc)
