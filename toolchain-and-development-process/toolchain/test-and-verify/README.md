# Test and Verify

## Tools to use the Kernel

If you upgrade your kernel to a version different from the one that came with your distribution, some of these packages may also need to be upgraded in order for the system to work properly.

1.  **util-linux**

    Most of these utilities handle the mounting and creation of disk partitions and manipulation of the hardware clock in the system. It is recommended that you install the latest version of this package, because new version support new features added to the kernel. Bind mounts are one example of an option in newer kernels, and a newer version of util-linux is needed in order to have them work properly. \
    To determine which version of the util-linux package you have on your system, run the following command: \
    `fdformat --version`

    If you wish to download and install the util-linux package yourself, you can find it at [http://www.kernel.org/pub/linux/utils/util-linux](http://www.kernel.org/pub/linux/utils/util-linux).\

2.  **module-init-tools**

    The module-init-tools package is needed if you wish to use Linux kernel modules. It is useful to compile device drivers as modules and then load only the ones that correspond to the hardware present in the system. \
    All Linux distributions use modules in order to load only the needed drivers and options for the system based on the hardware present, instead of being forced to build all possible drivers and options in the kernel in one large chunk. Modules save memory by loading just the code that is needed to control the machine properly. \
    The linker for the module is now built into the kernel, which makes the userspace tools quite small. \
    Older distributions have a package called modutils that does not work properly with the >=2.6 kernel. The module-init-toolspackage is what you need to get the >=2.6 kernel to work properly with modules.\
    &#x20;If you wish to download and install the module-init-tools package yourself, you can find it at [http://www.kernel.org/pub/linux/utils/kernel/module-init-tools](http://www.kernel.org/pub/linux/utils/kernel/module-init-tools). It is recommended that the latest version of this package be installed, as new features added to the kernel can be used by newer versions of this package. Blacklisting modules to prevent them from being auto- matically loaded by the udev package is one such option that is present in newer versions of module-init-tools, but not older ones. \
    To determine which version of the module-init-tools package you have on your system, run the following command:

    `depmod -V`\
    ``
3.  &#x20;**Filesystem-Specific Tools**

    A wide range of tools specific to particular filesystems are necessary to create, format, configure, and fix disk partitions. The _**util-linux**_** ** package has a few of these utilities, but some of the more popular file-systems have separate packages that contain the necessary programs.

    1.  **ext2/ext3/ext4**

        any recent version of an ext2-based tool can work with the other two filesystems as well. To work with any of these filesystems, you must have the _**e2fsprogs**_ package. \
        If you wish to download and install this package yourself, you can find it at [http://e2fsprogs.sourceforge.net](http://e2fsprogs.sourceforge.net/).\
        &#x20;It is highly recommended that you use the newest version in order to take advantage of newer features in the ext3 and ext4 filesystems. \
        To determine which version of e2fsprogs you have on your system, run the following command: \
        `tune2fs`
    2. **JFS**\
       ****To use the JFS filesystem from IBM, you must have the _**jfsutils**_ pacakge. If you wish to download and install this package yourself, you can find it at [http://jfs.sourceforge.net](http://jfs.sourceforge.net/). \
       To determine which version of jfsutils you have on your system, run the following command: \
       `fsck.jfs -V`
    3.  **ReiserFS**

        To use the ReiserFS file-system, you must have the _**reiserfsprogs**_ package. If you wish to download and install this package yourself, you can find it at [http://www.namesys.com/download.html](http://www.namesys.com/download.html). \
        To determine which version of reiserfsprogs you have on your system, run the following command: \
        `reiserfsck -V`
    4.  **XFS**

        To use the XFS filesystem from SGI, you must have the _**xfsprogs**_ package. If you wish to download and install this package yourself, you can find it at [http://oss.sgi.com/projects/xfs](http://oss.sgi.com/projects/xfs).

        To determine which version of xfsprogs you have on your system, run the following command:

        `xfs_db -V`
    5. **NFS**\
       ****To use the NFS filesystem properly, you must have the ** **_**nfs-utils**_ package. This package includes programs that let you mount NFS partitions as a client, and run an NFS server.( Some distributions, notably _Debian, call this package **nfs-common** instead of **nfs-utils**_.) \
       If you wish to download and install this package yourself, you can find it at [http://nfs.sf.net](http://nfs.sf.net/). \
       To determine which version of nfs-utils you have on yoursystem, run the following command: \
       `showmount --version`

    ****
4.  **Other-Tools**

    they enable access to different types of hardware and functions.

    ****

    1.  **udev**

        udev is a program that enables Linux to provide a persistent device-naming system in the /dev directory. It also provides a dynamic /dev, much like the one provided by the older (and now removed) devfs filesystem. Almost all Linux distributions use udev to manage the /dev directory, so it is required in order to properly boot the machine. Unfortunately, udev relies on the structure of /sys, which has been known to change from time to time with kernel releases. Some of these changes in the past have been known to break udev, so that your machine will not boot properly. If you have the latest version of udev recommended for your kernel and have problems with it working properly, please contact the udev developers on the mailing list available at [linux-hotplug-devel@lists.sourceforge.net](mailto:linux-hotplug-devel@lists.sourceforge.net). \
        It is highly recommended that you use the version of udev that comes with your Linux distribution, as it is tied into the distribution specific boot process very tightly. But if you wish to upgrade udev on your own, you can find it at [http://www.kernel.org/pub/linux/utils/kernel/hotplug/udev.html](http://www.kernel.org/pub/linux/utils/kernel/hotplug/udev.html). \
        To determine which version of udev you have on your system, run the following command: `udevainfo -V`
    2.  **Process tools**

        The package _**procps**_ includes the commonly used tools ps and top, as well as many other handy tools for managing and monitoring processes running on the system.\
        &#x20;If you wish to download and install this package yourself, you can find it at http://procps.sourceforge.net. \
        To determine which version of procps you have on your system, run the following command: \
        `$ ps --version`
    3.  **PCMCIA tools**

        In order to properly use PCMCIA devices with Linux, a userspace helper program must be used to set up the devices. For older kernel versions, this program was called pcmcia-cs, but that has been replaced with a much simpler system called _**pcmciautils**_. If you wish to use PCMCIA devices, you must have this package installed for them to work properly.

        If you wish to download and install this package yourself, you can find it at ftp://ftp.kernel.org/pub/linux/utils/kernel/pcmcia. \
        To determine which version of pcmciautils you have on your system, run the following command: \
        `$ pccardctl -V` \
        The latest version is recommended in order to take advantage of newer features in the PCMCIA subsystem, such as automatic driver loading when new devices are found.
    4. **Quotas**\
       ****To use the quota functionality of the kernel, you must have the _**quota-tools**_ package. This package includes programs that let you set quotas on users, provide statistics on the amount of quota being used by different users, and issue warnings when people get too close to using up their available filesystem quota.(Some distributions, notably Debian, call this package quota instead of quota-tools.) If you wish to download and install this package yourself, you can find it at [http://sourceforge.net/projects/linuxquota](http://sourceforge.net/projects/linuxquota). \
       To determine which version of quota-tools youhave on your system, run the following command: \
       `quota -V`

\
