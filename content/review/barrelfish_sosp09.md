---
title: "Barrelfish Review - SOSP '09"
date: 2020-01-15T07:31:37Z
# weight: 1
# aliases: ["/first"]
tags: ["distributed-system", "kernel", "operating-system", "SOSP"]
categories: ["paper-review"]
author: "Original"
# author: ["Me", "You"] # multiple authors
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
# description: "Desc Text."
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/mhxie/blog.mhxie.me/blob/main/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

## Summary
This paper presents a multi-kernel OS called Barrelfish for multicore hardware. The authors argued that current OS design and hardware-specific optimization could not exploit heteronomous cores nor suit the diverse, interconnected topology, and messages cost less than shared memory with the development of distributed or event-driven systems.

They designed the architecture of Barrelfish structured like a distributed system. It enforced explicit inter-core communication patterns to exploit the high-level knowledge to achieve more efficient cache-line updates. Another essential feature is that the OS should be agonistic to the underlying hardware by defining transport primitives and hardware interfaces. Finally, Barrelfish further reduces the communication overhead by using replications.

The most effective implementations of Barrelfish are its CPU drivers and monitors. CPU drivers are some event-driven, single-threaded, nonpreemtable processes that use interrupts to process the incoming events. Monitors are running at userspace to coordinate inter-core communications, including communication setup, processes wake-up to replicate and update data structures. There are still lots of detailed implementations to build the whole OS.

They compared the Barrelfish OS with the traditional Linux kernel and found it generally achieves comparable or better performance on TLB shootdown, messaging costs, and compute-bound workloads when the number of cores is large enough.


## Significance
This is the first multi-kernel Operating System that opens a new area of OS designs. Some new system has borrowed the ideas even part of the source codes from the Barrelfish like Popcorn, NotouchOS, HarmonyOS, etc. However, there is no major update on the Barrelfish OS since October 2018. 

## Questions

1. When speaking messages cost less than shared memory, does the access pattern for multiple cache-line accesses matter? Performing sequential updates might have the benefits of batching requests to reduce the overhead.
2. Why is Barrelfish no longer getting updates and less attractive than it was proposed?
3. This paper mainly describes and evaluates multi-kernel design for a single machine with multiple cores. What if we apply the idea of Barrelfish to a disaggregated infrastructures? As far as I know, there is a design called splitkernel proposed by Lego OS that disseminates kernel functions by different types of hardware components.


### Reference

[1] Baumann, A., Barham, P., Dagand, P. E., Harris, T., Isaacs, R., Peter, S., ... & Singhania, A. (2009, October). The multikernel: a new OS architecture for scalable multicore systems. In Proceedings of the ACM SIGOPS 22nd symposium on Operating systems principles (pp. 29-44).