---
layout: post
title:  "Native Docker like experience on Mac with Multipass"
date:   2022-11-04 22:30:12 +0000
categories: docker macos 
tag: docker docker-compose macos
---

Install multipass
```
brew install –cask multipass
```

Install the docker VM
```
multipass launch docker --cpus 4 --disk 200G --mem 16G
```

Install docker-compose on the image
Open shell on image


sudo apt install docker-compose


Set up SSH
- Open a shell in the docker vm
- Generate an ssh key
    - ssh-keygen -t ed25519 -C <your-email.com>
- Copy the id_ed25519.pub contents
- Paste contents into /home/ubuntu/.ssh/authorized_keys
- ssh ubuntu@<ip-address>

Set up your local machine to use the VMs docker (safer than exposing docker tcp)
```
docker context create multipass-docker --docker "host=ssh://ubuntu@192.168.64.2" --description "Used to connect to docker running on a multipass vm"
docker context use multipass-docker
```
