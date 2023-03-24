---
description: This page describes different tools used for Linux Kernel Development
---

# Development Tools

## Caution

1. Keep your experimental system different from your personal system where you store data which you can’t afford to loose. (You can use virtualbox or old computers for development purpose.)
2. Do not configure or build your kernel with superuser permissions enabled! - downloading the kernel source code, uncompressing it, configuring the kernel, and building it—should be done as a normal user on the machine. Only the two or three commands it takes to install a new kernel should be done as the superuser (root).
3. The kernel source code should also never be placed in the /usr/src/linux/ directory, as that is the location of the kernel that the system libraries were built against, not your new custom kernel. Do not do any kernel development under the /usr/src/ directory tree at all, but only in a local user directory where nothing bad can happen to the system.

## Configuring system for development

_****_[_**use a separate development machine when running development kernels.**_](#user-content-fn-1)[^1]_****_

* On development and test systems, it is a good idea to ensure there is ample space for kernels in the boot partition. Choosing a whole disk install or setting aside 5 GB disk space for the boot partition is recommended.
* Once the distribution is installed and the system is ready for development packages, enable the root account and also enable sudo for your user account.&#x20;
* The system might already have the _**build-essential**_ package, which is what you need to build Linux kernels on an x86-64 system. Recent Ubuntu distributions install a lot of the tools we will need.
* Execute following commands to install necessary packages\
  `sudo apt-get install build-essential vim git cscope libncurses-dev libssl-dev bison flex`\
  `sudo apt-get install vim libncurses5-dev gcc make git exuberant-ctags libssl-dev bison flex libelf-dev bc`\
  `sudo apt-get install git-email`
* Once you have a development system, it is time to check if your system supports the [_Minimal requirements to compile the Kernel_](https://www.kernel.org/doc/html/latest/process/changes.html); these change from time to time. It is a good idea to make sure your system is configured correctly.
* **grub configuration**
  * To make the grub boot menu always appear under Ubuntu

{% code overflow="wrap" lineNumbers="true" %}
```bash
sudo vim /etc/default/grub 
#(If you have the following lines in your grub file, you'll need to delete them)
GRUB_HIDDEN_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT_QUIET=true
#This next line controls how long grub shows the menu before it chooses the kernel at the top of the list (which is usually the most recent kernel):
GRUB_TIMEOUT=10
#You may want to increase the GRUB_DEFAULT timeout from 10 seconds to 30 seconds or more. I set it to 60 seconds.
#Also, check that
GRUB_TIMEOUT_STYLE=menu
#After you've finished editing the grub file, you need to tell grub that you made a change, so that can re-write some automatically generated files. Run this command:
sudo update-grub2
```
{% endcode %}



[^1]: 
