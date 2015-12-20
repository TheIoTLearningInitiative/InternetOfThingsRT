Internet of Things Real Time
==

## IoT Real Time 

This document describes the procedure of installing and using the Real Time Preemption patch for the Linux kernel, and discusses the first steps towards writing hard Real Time programs. It focuses on x86, as this is currently the most mature architecture.

This document will serve as a guidance for those whose purpose is to develop real time applications, having a Rea Time Preemption patch in the Linux Kernel. When talked about Real Time, the term Real Time Processing is pulled, when an output is immediately returned to the input. 

All procedures are done under the Intel Edison platform and a Linux Operating System, Debian 7.9. The code for the Pre-Setup for other distributions is shown, tested to be working, but not used in this document.

## About Real Time Preempt Patch

The standard Linux Kernel has no Real Time capabilities, it only meets soft Real Time requirements; the standard Linux Kernel provides basic time handling but does not guarantee hard timing deadlines.  

When we apply the RT-Preempt patch the system does not suddenly becomes a Real Time system, but it will be able to shorten the delay time-consuming context switch on the process that Real Time is specified.

- [Real-Time Linux Wiki](https://rt.wiki.kernel.org/index.php/Main_Page)

