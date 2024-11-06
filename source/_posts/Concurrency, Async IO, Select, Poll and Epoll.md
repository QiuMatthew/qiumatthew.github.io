---
title: Concurrency, Async IO, Select, Poll and Epoll
date: 2024-11-02
tags:
  - engineering
---

## Intro to Concurrency

If you studied computer science or engineering as an undergraduate, you've probably know that when we have multiple processes, our operating system help us manage the CPU time slices and have a mechanism to assign the computational resource to processes. 

This can be considered as an example of concurrency. Since we only have one processor but it can handle multiple processes. But this is not what we will discuss in this article.

What we will be focus on is, the computational resource is not always the biggest problem in concurrency. There are also I/O-bound tasks.

## The blocking nature of I/O operation

We know that I/O process can be very slow compared to CPU computation. When a process is ongoing an I/O process such as reading from or writing to the hard disk, this can take a rather long time, during which the process will be blocked, waiting for the I/O process to be finished.

But no one tells our operating system that the process is blocked by I/O operation, therefore it will still assign CPU to the blocked process and the process will be doing nothing and waste the valuable CPU resource.

In this case, the process does not need CPU time, but need the OS to inform them once the I/O is ready.

## Async I/O

The OS work on a preemptive basis (giving each thread or process CPU time), while async I/O is cooperative, meaning tasks yield control by waiting for an I/O event.

To achieve this, our OS kernel provides us system calls that allow programs to receive notifications about I/O readiness.

Of course this implicates that different OS would offer different system calls with different names, for example `IOCP` on Windows and `kqueue` on macOS. But there are three popular solution during the evolution of I/O multiplexing, `select`, `poll`, and `epoll` .

## Select

`select` is one of the oldest I/O multiplexing methods. You pass a set of file descriptors to `select`, along with a timeout. `select` will block until one of the descriptors is ready or until the timeout expires.

However, it has a limit on the maximum number of file descriptors it can handle, usually defined by `FD_SETSIZE`.

## Poll

`poll` was introduced to overcome some of the limitations of `select`. It accepts a list of file descriptors and allows more than `FD_SETSIZE` descriptors.

## Epoll

`epoll` is Linux-specific. You register file descriptors with `epoll`, and the kernel keeps track of them, notifying you only when they are ready for the requested operation. This avoids repeated polling of all descriptors.

## Summary

`select`, `poll`, and `epoll` are system calls provided by OS, they are async I/O methods that let I/O-bound tasks only react to I/O readiness and let a single thread be able to monitor multiple file descriptors (in fact, it is the OS that is taking the monitoring responsibility). Saving CPU resource in comparison to multi-threading.
