---
layout: post
current: post
cover: assets/images/lock_and_chain.jpg
navigation: True
title: Linux Server Hardening
date: 2018-05-11 21:58:30
tags: [security, linux]
class: post-template
subclass: 'post'
author: nvhaver
---
# Linux Server Hardening

## Physical System Security

### BIOS Password

Prevent strangers from booting your devices.

- Configure the BIOS to disable booting from CD/DVD, External Devices, Floppy Drive in BIOS.
- Enable BIOS password
- [Protect GRUB with password](https://www.tecmint.com/password-protect-grub-in-linux/)

## Partitioning

Having different partitions can prevent damage on one partition from spreading to other partitions. Third party applications should be installed on a separate file system under /opt. The following partitions are recommended: 

- /
- /boot
- /usr
- /var
- /home
- /tmp
- /opt

The /boot partition should be configured to be read-only. This can be done using `fstab`.

```
# vi /etc/fstab
LABEL=/boot     /boot     ext2     defaults,ro     1 2
```

Please note that you need to reset the change to read-write if you need to upgrade the kernel in future.

## Package management

To reduce the amount of vulnerabilities in the system, and thus the attack surface, the amount of applications, services and packages should be minimized.

### Services

Use the [`chkconfig`](https://www.tecmint.com/chkconfig-command-examples/) command to find out services which are running on runlevel 3. Once you’ve find out any unwanted service are running, disable them using the same command.

```
# /sbin/chkconfig --list | grep '3:on'
# /sbin/chkconfig serviceName off
```

TODO: Explain the chkconfig command
TODO: Explain [runlevels](https://www.liquidweb.com/kb/linux-runlevels-explained/) 

You can also look at the processes `ps -ef`


### Packages

The system's package manager can be used to list all the currently installed packages and to remove the ones which are no longer needed.

TODO: How do you know a package is no longer needed?

```
# yum -y remove package-name

# sudo apt-get remove package-name
```

TODO: Discuss the different package managers and how to use them 
- [RPM](https://www.tecmint.com/20-practical-examples-of-rpm-commands-in-linux/)
- [YUM](https://www.tecmint.com/20-linux-yum-yellowdog-updater-modified-commands-for-package-mangement/)
- [APT](https://www.tecmint.com/useful-basic-commands-of-apt-get-and-apt-cache-for-package-management/)

## Network ports

All connections on the system can be viewed by using the netstat command. The associated programs and services can then be stopped an disabled using the `chkconfig` command

```
# netstat -tulpn
```

TODO: Explain options used and add [examples](https://www.tecmint.com/20-netstat-commands-for-linux-network-management/). 

Alternatively, the following command can be used: `lsof -ni | grep LISTEN`

## SSH

To connect to the server in a secure way, SSH should be set up. It uses encryption for all traffic. Also the default port (22) should be changed to a higher level port number. Next to this, there are some configuration options which should be changed.
 
```
# vi /etc/ssh/sshd_config

PermitRootLogin no # Disable root Login
AllowUsers username # Only allow Specific Users
Protocol 2 # Use SSH Protocol 2 Version
```

Another thing to do is to set an SSH banner on login.

TODO: [Elaborate](https://www.tecmint.com/5-best-practices-to-secure-and-protect-ssh-server/)
TODO: [Banners](https://www.tecmint.com/protect-ssh-logins-with-ssh-motd-banner-messages/)
TODO: [SSH without passwords](http://www.linuxjournal.com/content/simple-server-hardening?page=0,1)

## Updates

You should always keep your system up to date with the latest security patches.

## Cronjobs

Cron has it’s own built in feature, where it allows to specify who may, and who may not want to run jobs. This is controlled by the use of files called /etc/cron.allow and /etc/cron.deny. To lock a user using cron, simply add user names in cron.deny and to allow a user to run cron add in cron.allow file. If you would like to disable all users from using cron, add the 'ALL' line to cron.deny file.

```
# echo ALL >>/etc/cron.deny
```

TODO: [Elaborate](https://www.tecmint.com/11-cron-scheduling-task-examples-in-linux/)

## Disable USB

By creating the file `/etc/modprobe.d/no-usb` and adding below line, USB devices will no longer be detected or installed by the system.

```
install usb-storage /bin/true
```

## SELinux

Security-Enhanced Linux (SELinux) is a compulsory access control security mechanism provided in the kernel. SELinux provides three basic modes of operation and they are:
- Enforcing: This is default mode which enable and enforce the SELinux security policy on the machine.
- Permissive: In this mode, SELinux will not enforce the security policy on the system, only warn and log actions. This mode is very useful in term of troubleshooting SELinux related issues.
- Disabled: SELinux is turned off.

You can view current status of SELinux mode from the command line using `system-config-selinux`, `getenforce` or `sestatus` commands.

```
# sestatus
# setenforce enforcing
```

## Graphical Interfaces

As we are taking about server machines here, there is no valid reason for not running it in headless mode. Therefore the packages for the graphical interface should be deleted. The server should only boot up to init level 3. This can be done by modifying the `/etc/inittab` file. To remove the packages, the package manager can be used.

```
# yum groupremove "X Window System"
```

## Network features

Some networking features can be deleted as well if they are not in use. As an example, IPv4 or IPv6 should be disabled when not in use.

```
# vi /etc/sysconfig/network

NETWORKING_IPV6=no
IPV6INIT=no
```

ICMP and broadcast requests can also be ignored by editing the `/etc/sysctl.conf` file.

```
Ignore ICMP request:
net.ipv4.icmp_echo_ignore_all = 1
Ignore Broadcast request:
net.ipv4.icmp_echo_ignore_broadcasts = 1
```

The new settings can be loaded by issuing `sysctl -p`

## Password Policy 

The weakest link in the security setup is still the human user. If passwords are being used (please try to find stronger alternatives) a password policy can be put in place to increase the overall strength of these passwords.

### Password Reuse

There are some Pluggable Authentication Modules (PAM) that can be used to enforce this policy.

TODO: Find the modules and elaborate on them.

### Password Expiration

The current expiration policy for a given user can be listed using the `chage` command. The same command can be used to set the listed parameters.

```
# chage -l username
Last password change					: Jan 18, 2018
Password expires					: never
Password inactive					: never
Account expires						: never
Minimum number of days between password change		: 0
Maximum number of days between password change		: 99999
Number of days of warning before password expires	: 7
```

### Password Strength

The `pam_cracklib` PAM module can be used to test password strength and enforce users to select stronger passwords.

### Empty Passwords
To check if there were any accounts with empty password, use the following command.

```
# cat /etc/shadow | awk -F: '($2==""){print $1}'
```

## Locking User Accounts

A user which is not currently in use can be locked down using the `passwd` command. Deep down, the command will replace the encrypted password with an exclamation mark, indicating that the account is locked. Attempting to log into the account will then result in an error message. The root user will however still have access to the account.

```
# passwd -l username
# passwd -u username
```

## Firewall

Iptables is the de-facto standard when it comes to firewalling in Linux.

TODO: [Elaborate](https://www.tecmint.com/basic-guide-on-iptables-linux-firewall-tips-commands/)

## Disable Ctrl+Alt+Delete

The behaviour of this key combination is defined in the `/etc/inittab` file. The line which defines it should be commented out.

```
# Trap CTRL-ALT-DELETE
#ca::ctrlaltdel:/sbin/shutdown -t3 -r now
```

TODO: [Iptables & ssh without password](http://www.linuxjournal.com/content/server-hardening?page=0,1)

## Monitoring

If you are dealing with lots of users, then its important to collect the information of each user activities and processes consumed by them and analyse them at a later time or in case if any kind of performance, security issues. But how we can monitor and collect user activities information.

There are two useful tools called 'psacct' and 'acct' are used for monitoring user activities and processes on a system. These tools runs in a system background and continuously tracks each user activity on a system and resources consumed by services such as Apache, MySQL, SSH, FTP, etc. For more information about installation, configuration and usage, visit the below url.

TODO: [Elaborate](https://www.tecmint.com/how-to-monitor-user-activity-with-psacct-or-acct-tools/)

## Logging

Move logs in dedicated log server, this may prevents intruders to easily modify local logs. Below are the Common Linux default log files name and their usage:

- /var/log/message - Where whole system logs or current activity logs are available.
- /var/log/auth.log - Authentication logs.
- /var/log/kern.log - Kernel logs.
- /var/log/cron.log - Crond logs (cron job).
- /var/log/maillog - Mail server logs.
- /var/log/boot.log - System boot log.
- /var/log/mysqld.log - MySQL database server log file.
- /var/log/secure - Authentication log.
- /var/log/utmp or /var/log/wtmp : Login records file.
- /var/log/yum.log: Yum log files.

## Backups

HA/DR measures should be implemented.

### NIC Bonding

There are two types of mode in NIC bonding, need to mention in bonding interface.

    mode=0 - Round Robin
    mode=1 - Active and Backup

NIC Bonding helps us to avoid single point of failure. In NIC bonding, we bond two or more Network Ethernet Cards together and make one single virtual Interface where we can assign IP address to talk with other servers. Our network will be available in case of one NIC Card is down or unavailable due to any reason.

TODO: [Elaborate](https://www.tecmint.com/create-nic-channel-bonding-in-redhat-centos-fedora/)



