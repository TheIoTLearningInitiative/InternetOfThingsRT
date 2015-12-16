Internet of Things Real Time
=======

## IoT Real Time 

This document describes the procedure of installing and using the Real Time Preemption patch for the Linux kernel, and discusses the first steps towards writing hard Real Time programs. It focuses on x86, as this is currently the most mature architecture.

This document will serve as a guidance for those whose purpose is to develop real time applications, having a Rea Time Preemption patch in the Linux Kernel. 

All procedures are done under the Intel Edison platform and a Linux Operating System, Debian 7.9. The code for the Pre-Setup for other distributions is shown, tested to be working, but not used in this document.


## About Real Time Preempt Patch

The standard Linux Kernel has no Real Time capabilities, it only meets soft Real Time requierements; the standard Linux Kernel provides basic POSIX operations for userspace time handling but 

About the RT-Preempt Patch
The standard Linux kernel only meets soft real-time requirements: it provides basic POSIX operations for userspace time handling but has no guarantees for hard timing deadlines. With Ingo Molnar's Realtime Preemption patch (referenced to as RT-Preempt in this document) and Thomas Gleixner's generic clock event layer with high resolution support, the kernel gains hard realtime capabilities.
The RT-Preempt patch has raised quite some interest throughout the industry. Its clean design and consequent aim towards mainline integration makes it an interesting option for hard and firm realtime applications, reaching from professional audio to industrial control.
As the patch becomes more and more usable and significant parts are leaking into the Linux kernel, we see the urgent need for more documentation. This paper tries to fill this gap and provide a condensed overview about the RT-Preempt kernel and its usage.
The RT-Preempt patch converts Linux into a fully preemptible kernel. The magic is done with:
Making in-kernel locking-primitives (using spinlocks) preemptible though reimplementation with rtmutexes:
Critical sections protected by i.e. spinlock_t and rwlock_t are now preemptible. The creation of non-preemptible sections (in kernel) is still possible with raw_spinlock_t (same APIs like spinlock_t)
Implementing priority inheritance for in-kernel mutexes, spinlocks and rw_semaphores. For more information on priority inversion and priority inheritance please consult Introduction to Priority Inversion
Converting interrupt handlers into preemptible kernel threads: The RT-Preempt patch treats soft interrupt handlers in kernel thread context, which is represented by a task_struct like a common userspace process. However it is also possible to register an IRQ in kernel context.
Converting the old Linux timer API into separate infrastructures for high resolution kernel timers plus one for timeouts, leading to userspace POSIX timers with high resolution.