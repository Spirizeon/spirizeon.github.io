
![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711290328715/4c1649c8-9201-4b40-9b43-ce64cddc2dde.png?w=1600&h=840&fit=crop&crop=entropy&auto=compress,format&format=webp)

# ❄️  NixOS: OS as Code
### Fundamental functionality and beauty of the Nix operating system

There are many Linux-based distributions out there, some are flashy, some are minimal, some are bloated (i guess we all hate those ones, lol). Then we have NixOS, a very fundamentally different Linux distro from all its other relatives. It can be summarized in three words: declarative, reproducible, and immutable.

## Declarative

Let's start with the first attribute, NixOS's declarative configuration makes it easy to write configurations for the various components in our operating system in an easy-to-understand language called "Nix". Nix is very similar to JSON syntax wise.
For declaring NixOS config, we write a file called configuration.nix which is stored in /etc/nixos/

```
{
  imports =
    [ # Include the results of the hardware scan.
      ./hardware-configuration.nix

    ];

  # Bootloader.
  boot.loader.systemd-boot.enable = true;
  boot.loader.efi.canTouchEfiVariables = true;

  networking.hostName = "endernix"; # Define your hostname...
  # Enable networking
  networking.networkmanager.enable = true;

  # Enable docker
  virtualisation.docker.enable = true;

  # Enable bluetooth
  hardware.bluetooth.enable = true;
  hardware.bluetooth.powerOnBoot = true;
...
}
```

The comments above each line or chunk of code describe their functionality. For example, the Bluetooth chunk describes that when re-building the system again, it needs to enable and start the bluetooth service on boot. We'll get into rebuilds later on, but how do we install packages? Easy. we declare it in the same file.

```
{ ...
users.users.ifkash = {
    isNormalUser = true;
    description = "Kashif";
    extraGroups = [ "networkmanager" "wheel" "docker" ];
    packages = with pkgs; [
      # Browsers
      brave
      firefox
      google-chrome
      # Fetch tools
      neofetch
      sysfetch
      nitch
    ];
};

...
}

```

Now these blocks can be placed anywhere in the file, because Nix is a purely functional programming language, which in layman's terms mean that positions of definitions does not matter sequentially.

## Reproducibility

Once our system's file has been configured, it's time to build/modify our system. Running sudo nixos-rebuild switch will read the entire configuration file and build a new Nix system accordingly. Now comes the question, would it take nearly double the size of the previous version if i simply include small packages in the new rebuild? The answer is No.
/nix/store

Nix store is a location where Nix system stores all the names of the packages in the form of files with their dependency list hashed and attached to the file name. This way, whenever the system is rebuilt, if it is available in nix store, it is simply ignored. If not, a new file is created in nix store and added with appropriate rules.

We can also go back to our previous build with `nixos-rebuild --rollback switch
Rollbacks - Never break your system again

Hence, incase our new system breaks for some reason, we can always switch back to the previous version and get our work done. This feature gives it the deadly advantage against distros like Arch Linux.

## Immutability

Since the system is defined declaratively, it's not possible to modify the system's existing state (like updating/upgrading packages, etc) This is not a bug, but a feature. It enables further safety against breaking our systems. This also means we cannot install additional things into the system in the current rebuild. It also makes the system more secure against attacks that intend to change ownership permissions or corrupt the boot files of the system.

### The Nix package manager

The Nix package manager fetches from the Nix package repository, one of the largest repos among all operating systems, it even beats Arch's user repository well known for its enormous package support. Hence, Nix boasts better software support than most operating systems on the planet.

You can search your favorite packages here: https://search.nixos.org/packages

### Why use NixOS as a daily driver?

Apart from excellent software support, and all its other features. NixOS doesn't feel different from other Linux distros at all on the surface level. There's that feel-at-home and works-out-of-the-box feeling. Now that NixOS's installation ISO comes with the graphical calamares installer, it makes installing it even easier.

## Acknowledgements

+ https://nixos.org/
+ https://github.com/kashifulhaque/nixos-config
+ https://ianthehenry.com/posts/how-to-learn-nix/
+ https://www.youtube.com/@vimjoyer

---

# 🩸 Uniqueness of Arch Linux

The blog focuses heavily on Linux, especially Arch, because it is one of the most minimal and hence most universally adaptable distribution of Linux. Arch based distributions also come with some superpowers that other distros like debian don't provide. Let's take a deep dive into those exquisite abilities on this day.

### Arch Wiki

The ultimate manual for anything arch-related. The arch wiki and arch forums have everything you need regarding any issue you're facing or any mods you want to do to arch. Let's say you want to install gnome on your Arch that runs KDE-Plasma but don't exactly know how to do it. Just put in your issue with the suffix "arch wiki" in google.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705827310455/1096c746-40cc-4a1f-872a-b1222ce8d76a.png)

The instructions rarely get outdated as the wiki is regularly updated with the latest information by the community (which is public and open). Some times the solutions proposed in arch wiki pages even work on non-arch based distributions if it is not a distro-specific problem.

### Arch User Repository (AUR)

Apart from The community maintained registry for arch linux packages. It is ginormous in terms of variety of packages. You can install almost any package from the AUR, be it Spotify, Discord or even Google-Chrome!

#### AUR helpers

Installing a package from the AUR, requires you to clone the repo and give the build command `makepkg -si` so that the system reads the MAKEPKG file.

MAKEPKG acts as a blueprint for building the package and adding it to system PATH.

However, AUR helpers assist in automating the build process. There are several apps like `yay` and `paru`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705828073036/37633216-0338-45cc-bfa7-b271b32be751.png)

### Minimalism

Arch Linux provides us with the power to start from a minimal installation, which means that we decide what goes into our system, including even the kernel!

This helps avoid bloatware and enable for a lightning-fast system. Especially for developers when they combine their workflow with the clax.nvim neovim configuration.

### Self-development

Not only do we learn about partitioning, and different kinds of file systems, keymapping, audio servers and daemons, but also does installing arch help build a basic overview knowledge of foundations of an operating system.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705828104991/d94fcd76-5f5b-4add-b28c-11bcdd642492.png)

### Bleeding-edge updates

This is also a double-edged sword, bleeding-edge updates means that all of the system's packages are updated to the very latest version. If neovim's version on Debian is 0.6.1, it is 0.9.1 on Arch linux. But it also means that despite supporting newer technologies like lua configs in neovim, software and hence system may risk breaking due to the packages being too new and not being compatible with things like hardware.

Maintenance is also needed in Arch Linux, so we need to regularly make sure the system is updated. This is not an issue because it helps us stay more conscious of our system's workings.

## Security Implementations

In an era where digital threats are constantly evolving, prioritizing the security of your system is paramount. This blog will delve into various aspects of enhancing security, covering essential topics such as drive encryption, file systems, firewalls, and secure boot. By incorporating robust security measures, you can significantly reduce the risk of unauthorized access, data breaches, and other potential threats.

#### **Drive Encryption with**`cryptsetup`**:**

Drive encryption is a fundamental step towards securing your data. The use of `cryptsetup`, a utility for setting up encrypted filesystems, ensures that even if someone gains physical access to your storage device, they won't be able to access sensitive information without the encryption key.

To implement drive encryption with `cryptsetup`, follow these steps:

* Install `cryptsetup` on your system.
    
* Use the `cryptsetup` command to create an encrypted container or encrypt an existing partition.
    
* Set up a strong passphrase or keyfile for added security.
    

**Understanding**`btrfs`**File System:**

The choice of a file system plays a crucial role in system security. The Btrfs (B-Tree File System) is a modern and feature-rich file system that offers advantages over traditional ones like ext4. `btrfs` provides improved data integrity, efficient snapshots, and support for advanced storage technologies.

Compare `btrfs` with `ext4`:

* `btrfs` supports advanced features like snapshots, copy-on-write, and checksums.
    
* `ext4` is a mature and stable file system with a long history of reliability.
    
* Consider your specific use case and requirements before choosing between `btrfs` and `ext4`.
    

#### **Configuring ufw Firewall:**

A firewall is a critical component in safeguarding your system from network threats. Uncomplicated Firewall (ufw) is a user-friendly interface for managing iptables, the default Linux firewall. By enabling ufw, you can control incoming and outgoing traffic, thereby fortifying your system against potential attacks.

To set up `ufw`:

* Install `ufw` on your system.
    
* Define rules for allowing or blocking specific traffic.
    
* Enable `ufw` to start on system boot for continuous protection.
    

#### **Secure Boot with**`sbctl`**:**

Secure Boot is a security feature that ensures the integrity of the boot process, preventing the loading of unauthorized or tampered code during system startup. `sbctl` is a tool for managing Secure Boot settings on Linux systems.

To enable Secure Boot with `sbctl`:

* Check if your system supports Secure Boot.
    
* Put Secure boot in setup mode by deleting all secure boot variables.
    
* Generate and enroll Secure Boot keys.
    
* Use `sbctl` to configure and enable Secure Boot.
    

In today's digital landscape, prioritizing security is non-negotiable. By implementing robust measures such as drive encryption, choosing advanced file systems like Btrfs, configuring firewalls, and enabling Secure Boot, you significantly enhance the protection of your system. RIn an era where digital threats are constantly evolving, prioritizing the security of your system is paramount. This blog will delve into various aspects of enhancing security, covering essential topics such as drive encryption, file systems, firewalls, and secure boot. By incorporating robust security measures, you can significantly reduce the risk of unauthorized access, data breaches, and other potential threats.

#### **Drive Encryption with**`cryptsetup`**:**

Drive encryption is a fundamental step towards securing your data. The use of `cryptsetup`, a utility for setting up encrypted filesystems, ensures that even if someone gains physical access to your storage device, they won't be able to access sensitive information without the encryption key.

To implement drive encryption with `cryptsetup`, follow these steps:

* Install `cryptsetup` on your system.
    
* Use the `cryptsetup` command to create an encrypted container or encrypt an existing partition.
    
* Set up a strong passphrase or keyfile for added security.
    

**Understanding**`btrfs`**File System:**

The choice of a file system plays a crucial role in system security. The Btrfs (B-Tree File System) is a modern and feature-rich file system that offers advantages over traditional ones like ext4. `btrfs` provides improved data integrity, efficient snapshots, and support for advanced storage technologies.

Compare `btrfs` with `ext4`:

* `btrfs` supports advanced features like snapshots, copy-on-write, and checksums.
    
* `ext4` is a mature and stable file system with a long history of reliability.
    
* Consider your specific use case and requirements before choosing between `btrfs` and `ext4`.
    

#### **Configuring ufw Firewall:**

A firewall is a critical component in safeguarding your system from network threats. Uncomplicated Firewall (ufw) is a user-friendly interface for managing iptables, the default Linux firewall. By enabling ufw, you can control incoming and outgoing traffic, thereby fortifying your system against potential attacks.

To set up `ufw`:

* Install `ufw` on your system.
    
* Define rules for allowing or blocking specific traffic.
    
* Enable `ufw` to start on system boot for continuous protection.
    

#### **Secure Boot with**`sbctl`**:**

Secure Boot is a security feature that ensures the integrity of the boot process, preventing the loading of unauthorized or tampered code during system startup. `sbctl` is a tool for managing Secure Boot settings on Linux systems.

To enable Secure Boot with `sbctl`:

* Check if your system supports Secure Boot.
    
* Put Secure boot in setup mode by deleting all secure boot variables.
    
* Generate and enroll Secure Boot keys.
    
* Use `sbctl` to configure and enable Secure Boot.
    

In today's digital landscape, prioritizing security is non-negotiable. By implementing robust measures such as drive encryption, choosing advanced file systems like Btrfs, configuring firewalls, and enabling Secure Boot, you significantly enhance the protection of your system. Regularly updating and auditing these security measures will ensure that your defenses remain strong against evolving threats.

### Documentations

* [https://docs.kernel.org/filesystems/btrfs.html](https://docs.kernel.org/filesystems/btrfs.html)
    
* [https://wiki.archlinux.org/title/dm-crypt/Device\_encryption](https://wiki.archlinux.org/title/dm-crypt/Device_encryption)
    
* [https://wiki.archlinux.org/title/Unified\_Extensible\_Firmware\_Interface/Secure\_Boot](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot)
    
* [https://docs.kernel.org/filesystems/btrfs.html](https://docs.kernel.org/filesystems/btrfs.html)
    
* [https://wiki.archlinux.org/title/dm-crypt/Device\_encryption](https://wiki.archlinux.org/title/dm-crypt/Device_encryption)
    
* [https://wiki.archlinux.org/title/Unified\_Extensible\_Firmware\_Interface/Secure\_Boot](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot)

---

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
