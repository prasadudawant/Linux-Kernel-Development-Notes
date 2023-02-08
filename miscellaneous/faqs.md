---
description: >-
  this page has lists some problems and faced which are quite common and
  solution to them
---

# FAQs

## Linux Kernel Module

1.  "build" directory not present for linux kernel version in lib/modules/\<kernel-name>, which is required to build linux kernel module.\
    ANS :- execute command if build directory is not present for currently executing kernel

    ```bash
    sudo apt install linux-headers-<kernel version no>
    #e.g., sudo apt linux-headers-4.15.0-202-generic
    ```
