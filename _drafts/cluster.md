---
layout: post
current: post
cover: assets/images/motherboard.jpg
navigation: True
title: Compute Cluster
date: 2018-05-12 18:10:30
tags: Programming
class: post-template
subclass: 'post'
author: nvhaver
---
# Compute Cluster

- Create inventory list of acquired hardware
- Install OS's for clustering: headless Linux (Ubuntu?)
- Configure the different nodes to be part of the same Beowulf cluster
- Set up an identical user account on all machines
- Assign one node as the master node
- Configure the openSSH server configuration
- Copy SSH pubkeys after generate to other nodes (ssh-copy-id user@ip) and disable password login

## Network Setup

For the network setup, the following steps will be taken:

- Configure the network to assign static IPs
- Add the other devices' IPs to the hosts files

## Resources

- [Wiki](https://en.wikipedia.org/wiki/Beowulf_cluster)
- [Old tutorial](https://www.linux.com/blog/building-beowulf-cluster-just-13-steps)
- [Official site](http://www.beowulf.org/overview/faq.html)
- [Wikibooks](https://en.wikibooks.org/wiki/Building_a_Beowulf_Cluster)
