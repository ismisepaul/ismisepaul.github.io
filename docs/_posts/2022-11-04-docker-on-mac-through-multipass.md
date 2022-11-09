---
layout: post
title:  "Native Docker Like Experience on Mac with Multipass"
date:   2022-11-04 22:30:12 +0000
categories: docker macos 
tag: docker docker-compose macos
---
Moving from Ubuntu to macOS meant losing native support for Docker but luckily [Multipass](https://multipass.run/)
came to the rescue to give a native like experience.

Multipass is a lightweight VM manager for Linux, Windows and macOS. It uses KVM on Linux, Hyper-V on Windows and 
HyperKit on macOS to run the VM with minimal overhead. https://github.com/canonical/multipass


Features 

- Works with Apple M1
- Easy to set up
- Volume mounting and networking is intuitive
- Limited to guest Ubuntu OS


## Installation

```
brew install –cask multipass
```

### Install the VM 

I do a lot with Docker, so I gave the VM a lot of resource (adjust as needed). Bear in mind I previously set this up 
with fewer resources and had to delete that VM as there's only a [workaround](https://github.com/canonical/multipass/issues/62) 
to adjust the size of the disk.

Multipass has a ready-made VM / [workflow](https://ubuntu.com/blog/docker-on-mac-and-windows-multipass?_ga=2.217931027.447270926.1668021980-112168335.1668021980) 
with Docker and Portainer called `docker`. You can create it with

```
multipass launch docker --cpus 4 --disk 200G --mem 16G
```

Get more info on your VM

```console
multipass info docker
```
```console
Name:           docker
State:          Running
IPv4:           192.168.64.2
Release:        Ubuntu 22.04.1 LTS
Image hash:     c63b65d7b495 (Ubuntu 21.10)
Load:           0.12 0.15 0.16
Disk usage:     18.9G out of 193.6G
Memory usage:   1.3G out of 15.6G
```

### Install docker and docker-compose on macOS

```console
brew cask install docker
```

```console
brew install docker-compose
```

Docker will try to connect to the Docker Engine but will be unsuccessful. Try it by running the following

```console
docker ps
```
```
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

## Setup

### Set up Passwordless SSH Access to the VM

This part is important as it'll play a key role in the native docker like experience.

1) Ensure you have generated an SSH key, if not `ssh-keygen -t ed25519 -C <YOUR-EMAIL>`
2) Get the IP of your VM `multipass list`
```console
➜  ~ multipass list
Name                    State             IPv4             Image
docker                  Running           192.168.64.2     Ubuntu 21.10
```
3) Copy your SSH public key to the clipboard `cat .ssh/id_ed25519.pub | pbcopy`
4) Get a shell on the VM `multipass shell 192.168.64.2` 
5) Paste your SSH public key to the VM `echo <COPIED PUBLIC KEY> >> .ssh/authorized_keys`
6) Now try SSH to the VM `ssh ubuntu@192.168.64.2` you should log in without being promoted for a password

### Set the Docker Context

You can use the docker command to connect to remote docker instances e.g. cloud providers or in this case the Docker 
engine running on the Multipass VM.

Within the macOS console run the following

```console
# Create the context 
docker context create multipass-docker --docker "host=ssh://ubuntu@192.168.64.2"
```

```console
# Verify the context has been created 
docker context list
```

```console
# Set the context 
docker context use multipass-docker
```

```console
# Run the following to verify - should output "CONTAINER ID IMAGE" etc.
docker ps
```

You're done go ahead and pull images or whatever else you need to do from the macOS console.


## GUI Interface

[Portainer](https://www.portainer.io/) comes preinstalled with the `docker` read-made (workflow) [Docker VM for Multipass](https://multipass.run/docs/docker-tutorial)

Run Portainer with

```console
docker run -d -p 9000:9000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

- In your browser go to `192.168.64.2:9000` check you multipass VM IP 
- You could also make it an app and have it in your docker using Chrome `Three Dots` -> `More Tools` ->
`Create Shortcut` -> Give it a name and tick the box `Open as Window`


## Applications with Content Security Policy (CSP)

When running Docker applications which have a CSP you can't load them using the VMs IP. For this you can use SSH to 
do local forwarding and access them through `localhost`. For example OWASP Dependency Track

```bash
# Downloads the latest Docker Compose file
curl -LO https://dependencytrack.org/docker-compose.yml

# Starts the stack using Docker Compose
docker-compose up -d
```

Set up two SSH local forwards from you mac to the VM

```bash
# For the Dependency Track UI
ssh -L 8080:localhost:8080 ubuntu@192.168.64.2
# For the Dependency Track API
ssh -L 8081:localhost:8081 ubuntu@192.168.64.2
```

You can now hit the application using `http://localhost:8080`

# Summary

- [Mutlipass](https://multipass.run/) can be used on Mac to give you a docker native experience
- Setting up passwordless SSH to the Multipass VM is essential to achieve this
- Install macOS docker and docker-compose
- Set the docker [context](https://docs.docker.com/engine/context/working-with-contexts/) to point to the VMs Docker 
engine
- You'll be able to run docker commands from macOS that seamlessly interacts with the Docker engine on the Multipass VM
