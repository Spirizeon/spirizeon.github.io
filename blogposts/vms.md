# 🤖 Virtual Machines and Containers

One of the most essential and important concerning isolated environments and modern cloud architecture in data centers. Virtual Machines and Containers provide a replication development/deployment environment for software. Despite both existing as an abstraction layer above the host system, they differ conceptually when concerning factors like their architecture, speed and resource footprint.

### Virtual Machines

Virtual Machines are specialized environments that emulate a computer operating system. They can be further specialized to deploy particular software. Let's think of running an OS like Linux, but over the host system running Windows. Here, Linux will be treated as a "Guest OS" and will be running atop Windows as an Application.

Virtual machines are governed by a type of software called a "Hypervisor", these can control their deployment.

![Understanding a hypervisor the simple way - Data Storage Solutions](https://www.data-storage.uk/wp-content/uploads/2022/03/hypervisor.jpg)

Hypervisors also enable us to distribute hardware resources among virtual machines according to preference. If we have a 128GB RAM server and a lot of clients that want to access it, we would split the hardware resource among several virtual machines, so that each client gets their own isolated space and OS to run with a particular quantity of machine resources (like CPU and memory) allotted to them.

### What can be improved

![What Is an OS Kernel? | Baeldung on Computer Science](https://www.baeldung.com/wp-content/uploads/sites/4/2021/05/os-kernel-2.png)

An Operating system in general consists of two stacked layers over the hardware.

Going from top to bottom, we first encounter the OS Applications layer, which consists of all the software run on the machine.

Secondly, we get the OS kernel, the layer that deals with communication with hardware resources, and acts as a bridge between the hardware and the OS Applications layer.

A virtual machine emulates both the OS kernel and the applications layer stacked in the mentioned order. This results in excessive use of resources, however there is a solution to this. We can use the host's kernel instead of having multiple kernels run altogether.

### Enter: Containers

With the added advantage of using the host's kernel, Containers save on resources and boost up speed. Docker is used specifically because it's one of the most popular tools and also the one that revolutionized this category, also its registry, Docker-Hub is one of the largest repositories for pulling dependencies for your project.

![Docker vs Virtual Machines (VMs) : A Practical Guide to Docker Containers  and VMs](https://images.contentstack.io/v3/assets/blt300387d93dabf50e/bltb6200bc085503718/5e1f209a63d1b6503160c6d5/containers-vs-virtual-machines.jpg)

We have a container engine that manages these containers or isolated environments, Just like a hypervisor.

## Spinning up a new container

Docker can be installed through the official set of instructions on their [site](https://docs.docker.com/engine/install/).

It uses the Docker engine which the docker daemon responsible for all processes concerning it.

There are two options, one is to either pull an existing container through Docker's registry called [Docker Hub](https://hub.docker.com/). Secondly is to build custom images to suit our project's needs.

### Using existing images

To clone a docker container off Docker Hub,

```dockerfile
docker pull <image_name>:<version>
```

We can then start this container by getting the image ID through the `docker images` command.

```dockerfile
docker start <image_ID>
```

### Building a custom image

In order to build a custom image, a blueprint file called a `Dockerfile` needs to be added to the project directory. This will contain all the configuration code, regarding what dependency versions and base image (which is the OS image that will be emulated) will be included in the container.

A typical `Dockerfile` would look somewhat similar to the one below.

```dockerfile
FROM <primary_service>:<base_image>
COPY <source code directory> /app/
WORKDIR <the working directory>
RUN <any terminal commands>
```

Now that the blueprint is made, time to build and deploy a docker container with the following configuration.

```dockerfile
docker build <location_to_save_container>
```

This builds the docker container following the blueprint, now this can be uploaded to Docker Hub or any repository to ship it to other environments and servers.

### Week 2 Extra: Docker Vs. Podman

Podman is a popular alternative to Docker and possibly is aiming to succeed it. It has now been adopted to be used in container orchestration tools like Kubernetes.

#### The Advantage

* Podman optionally requires root/admin privileges to build/run containers. Which makes it safer for the deployment system.
    
* Unlike Docker which uses a daemon (background service) that constantly listens for new requests using the client-server architecture. Podman's philosophy has a daemon-less architecture and hence saves up on system resources.
    
* Mostly all docker commands work with Podman with the replacement of the word "docker" with "podman" (at least as far as the CLI is concerned).
    
* Podman isn't a monolithic piece of software unlike Docker. It depends on software like systemd. Hence it is more lightweight.
    

In summary, Podman is overall a safer alternative than Docker.

### Some Extra Reads:

* [https://www.imaginarycloud.com/blog/podman-vs-docker/](https://www.imaginarycloud.com/blog/podman-vs-docker/)
    
* [https://berzi.hashnode.dev/containers-and-virtualization-with-docke](https://berzi.hashnode.dev/containers-and-virtualization-with-docker)r
    
* [https://docs.podman.io/en/latest/](https://docs.podman.io/en/latest/)
    
* [https://www.vmware.com/in/topics/glossary/content/virtual-machine.html](https://www.vmware.com/in/topics/glossary/content/virtual-machine.html)
