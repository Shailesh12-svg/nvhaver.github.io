---
layout: post
current: post
cover: assets/images/server.jpg
navigation: True
title: Virtual Machines
date: 2018-05-12 18:10:30
tags: infrastructure
class: post-template
subclass: 'post'
author: nvhaver
---
# Virtual Machines

In today's blog we'll be looking at virtualisation. I will provide a comparative study between the different options available, their benefits, drawbacks and which one I prefer.

## Microsoft Hyper-V

Hyper-V is provided out of the box with every Windows Pro license and as such is readily available in a corporate environment.

Hyper-V has a somewhat different architecture when compared to it's competitors. Instead of running the hypervisor as an application on top of the host operating system, it turns the host into a priviledged guest by installing itself under the host. This means that the host actually is a guest who shares its resources with the other guests, including the network interfaces.

A big drawback of Hyper-V which I discovered while using it in an enterprise setting is its disk usage. For every snapshot you take of a guest, the entire virtual disk is copied. As a result, it is impractical to use Hyper-V as you would either be battling it for disk space or shying away from taking snapshots, neither of which is acceptable at work. 

## VMWare

## Virtualbox

Up until now, Virtualbox has been my go-to virtualisation solution. The reason for this is that it is completely free to use. That being said, it does have some limitations. 
