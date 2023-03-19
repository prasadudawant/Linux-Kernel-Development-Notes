---
description: >-
  Debugging Analysis of Kernel panics and Kernel oopses using System Map and
  objdump
---

# Kernel Panics and oops

## Introduction

* **kernel panic -** It **** is an action taken by an operating system upon detecting an internal fatal error from which it cannot safely recover and force the system to do a controlled system hang / reboot due to a detected run-time system malfunction (not necessarily an OOPS). The operation of the panic kernel feature may be controllable via run-time sysconfig settings such as hung task handling. This is a kernel panic.
* **OOPS** - They are due to the Kernel exception handler getting executed when kernel finds something faulty(e.g., an invalid instruction) in kernel code. It’s somewhat like the segfaults of user-space. Each exception has a unique number. Some “oops”es are bad enough that the kernel decides to execute the panic feature to stop running immediately. This is a kernel crash optionally followed by invoking a panic.\
  The offending process that triggered this Oops gets killed without releasing locks or cleaning up structures. The system may not even resume its normal operations sometimes; this is called an unstable state. Once an Oops has occurred, the system cannot be trusted any further.
*   OOPS messages - When a Kernel OOPS is encountered in a running kernel an OOPS message  is displayed on the screen. The OOPS message contains the following: \
    summary of error occurred,  the address of the function that invoked the failure i.e  instruction pointer, the values of the CPU registers, PC, the stack, pid and the name of the current process executing, call trace, number of times oops occurred , error code and on which cpu oops occurred.

    The Tainted flag in oops log has its own meaning. A few other flags, and their meanings, picked up from `kernel/panic.c`:

    * `P` — Proprietary module has been loaded.
    * `F` — Module has been forcibly loaded.
    * `S` — SMP with a CPU not designed for SMP.
    * `R` — User forced a module unload.
    * `M` — System experienced a machine check exception.
    * `B` — System has hit bad\_page.
    * `U` — Userspace-defined naughtiness.
    * `A` — ACPI table overridden.
    * `W` — Taint on warning
* **refer** [**https://www.opensourceforu.com/2011/01/understanding-a-kernel-oops/**](https://www.opensourceforu.com/2011/01/understanding-a-kernel-oops/)****

## Disassembling the kernel

1. {% code overflow="wrap" %}
   ```bash
   objdump -D -S --show-raw-insn --prefix-addresses --line-numbers vmlinux > objdump
   ```
   {% endcode %}
2. {% code overflow="wrap" %}
   ```bash
   arm-none-linux-gnueabi-objdump –dr vmlinux  /*If We have object code handy then, we can disassemble the individual object file also like objdump -S panic.o"
   ```
   {% endcode %}
3. {% code overflow="wrap" %}
   ```bash
   arm-none-linux-gnueabi-gdb –silent vmlinux
   ```
   {% endcode %}

## System.map

* In Linux, the [System.map ](https://en.wikipedia.org/wiki/System.map)file is a symbol table used by the kernel. symbol table is nothing but Identifiers/Symbols used in Linux kernel and it's virtual address mapping.
* The kernel does the address-to-name translation itself when CONFIG\_KALLSYMS is enabled.
* &#x20;Addresses inside System.map may change from one build to the next or in another word new System.map is generated for each build of the kernel however it is must to have System.map of the same Linux kernel and .config on which Kernel panics/oopses has been reported to debug the problem.
* In the kernel backtraces in the logs, the kernel finds the nearest symbol to the address being analysed. Not all function symbols are available because of inlining, static, and optimisation so sometimes the reported function name is not the location of the failure.

## **How to Debug Kernel panics and oopses:**

The running kernel should be compiled with **`CONFIG_DEBUG_INFO`**, and `syslogd` should be running.

### with System.map and objdump&#x20;

1. Get the symbol/function name/ call stack from kernel backtrace message which you can find in dmesg log when kernel oops has occurred. ((Actually nearest function symbol to the crash).
2. Grep/find symbol in System.map file and note down symbol name and address.
3. Kernel oops log will hap information about CU register. check symbol address where crash has happened matched with pc register from kernel oops log. It will confirm you are using correct  System.map.
4.  run objdump on vmlinux to get the disassembly\
    [**objdump**](http://en.wikipedia.org/wiki/Objdump)**:** is a program for displaying various information about object files. For instance, it can be used as a [disassembler](http://en.wikipedia.org/wiki/Disassembler) to view executable in assembly form.

    [**vmlinux**](http://en.wikipedia.org/wiki/Vmlinux)**:** is a statically linked executable file that contains the [Linux kernel](http://en.wikipedia.org/wiki/Linux\_kernel) in one of the object file formats supported by Linux, The _vmlinux_ file might be required for kernel debugging, symbol table generation or other operations.\
    `#objdump -D -S --show-raw-insn --prefix-addresses --line-numbers vmlinux > objdump`
5. Find symbol in **vmlinux.objdump** and **** look for PC address calculated above.
6. You will find instruction where crash has happened. Analyze it.
7. Refer [https://sanjeev1sharma.wordpress.com/tag/debug-kernel-panics/](https://sanjeev1sharma.wordpress.com/tag/debug-kernel-panics/)

### **Using GDB to find the location where your kernel panicked or oopsed.**

1. Get the call stack from kernel panic/oops message which you can find in dmesg log when kernel oops has occurred. ((Actually nearest function symbol to the crash).
2. cd to your directory of your kernel tree and run gdb on the “.o” file which has the function  and use the gdb “list” command  and gdb should tell you the line number where you hit the panic or oops.\
   `gdb <path to oops module.ko>`\
   (`gdb) list *(function+0xoffset)`\
   ``e.g., in this case function is sd\_remove() and offset is 0x20 then \
   `(gdb)list *(sd_remove+0x20)`
3. You will find instruction where crash has happened. Analyze it.
