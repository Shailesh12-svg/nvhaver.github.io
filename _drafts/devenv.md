---
layout: post
current: post
cover: assets/images/code.jpg
navigation: True
title: Ultimate Development Environment
date: 2018-05-12 18:10:50
tags: Programming
class: post-template
subclass: 'post'
author: nvhaver
---
# Goal
Throughout the years there have been some tools and technologies which I always include in every development machine I create. This project aims to collect these tools and to automate the installation procedure for setting up a new machine.

# Requirements

- Linux for low level development and kick-ass cli kung fu.
- Windows for C#/.Net development.
- The two OS's need to be able to share project code and files.
- Language support for as many languages as possible.
- Includes IDE's: Netbeans, Eclipse, Android Studio, Visual Studio, Texmaker etc.

As a base for the ultimate development machine, I decided to go for a dual boot solution. Windows 10 and Ubuntu 16.04 LTS currently are my best choice, however I will look into replacing Ubuntu with a more flexible and advanced Linux distribution, such as Arch or compose a Linux from scratch.

To share files between the two platforms, I opted to create a separate NTFS formatted partition, as Windows still doesn't support the EXT4 filesystem. In order to prevent duplication of data and source code, I will create some symbolic links to this shared partition. However, I will not move the entire /home folder onto the shared partition as NTFS does not allow the same file level permissions as EXT4.


