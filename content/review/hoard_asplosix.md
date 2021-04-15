---
title: "Hoard Review - ASPLOS IX"
date: 2019-05-22T07:31:37Z
# weight: 1
# aliases: ["/first"]
tags: ["memory-allocator", "operating-system", "concurrency", "LIFO"]
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

Hoard is a concurrent memory allocator algorithm that achieves both a performance close to the uniprocessor allocator and a low memory fragmentation rate. False sharing and fragmentation are two showing problems that degrade the performance of concurrent memory allocators. Allocator may actively or passively cause false sharing by splitting a cache line to different processes in turn or by reusing the free pieces. On the other hand, internal and external fragmentation may cause unbounded memory blowup. Hoard assigns a local heap for each processor and maintains a global heap. All the memory in heaps is managed in a structure called superblock with the same size and exploits the locality by using LIFO free list. In the runtime, all the per-processor heaps would keep the emptiness under a given threshold. The freest superblock would be delivered to the global heap if the emptiness is too high. When calling malloc, the space of the lease free superblock would be used. Overall, with bounded memory overhead, Hoard make a concurrent allocator fast and salable.

## Strengths
* As fast as a uniprocessor allocator;
* The memory consumption is theoretically and experimentally bounded;
* Largely alleviate the false sharing problem.

## Weaknesses
* Lock overhead and global heap contention;
* The false sharing problem has not been totally removed;
* Deliver the large objects management to virtual memory system.

## How to improve
* Design a lock-free heap framework that further reduce the overhead of synchronization, especially for single-thread cases;
* Internal fragmentation for smalle objects could be further reduced by a cleaning daemon.

## Related work
The following works also focus on improving the performance of concurrent memory allocator, but from different perspectives. LAMA (ATC ’15) [2] and Streamflow (ISMM ’06) [3] want to improve the performance by exploiting cache locality; This work (PLDI ’04) [4] uses a lock-free implementation to improve Hoard, and FreeBSD malloc(3) (2006) [5] shares the same idea; more recent work like Mallacc (ASPLOS ’17) [6] and SuperMalloc (ISMM ’15) [7] are pushing the performance to a new level.


### Reference

[1] Emery D. Berger, Kathryn S. McKinley, Robert D. Blumofe, and Paul R. Wilson. 2000. Hoard: a scalable memory allocator for multithreaded applications. In Proceedings of the ninth international conference on Architectural support for programming languages and operating systems (ASPLOS IX). ACM.

[2] Hu, X., Wang, X., Li, Y., Zhou, L., Luo, Y., Ding, C., ... & Wang, Z. (2015). {LAMA}: Optimized Locality-aware Memory Allocation for Key-value Cache. In 2015 {USENIX} Annual Technical Conference ({USENIX}{ATC} 15) (pp. 57-69).

[3] Schneider, S., Antonopoulos, C. D., & Nikolopoulos, D. S. (2006, June). Scalable locality-conscious multithreaded memory allocation. In Proceedings of the 5th international symposium on Memory management (pp. 84-94). ACM.

[4] Michael, M. M. (2004, June). Scalable lock-free dynamic memory allocation. In ACM Sigplan Notices (Vol. 39, No. 6, pp. 35-46). ACM.

[5] Evans, J. (2006, April). A scalable concurrent malloc (3) implementation for FreeBSD. In Proc. of the bsdcan conference, ottawa, canada.

[6] Kanev, S., Xi, S. L., Wei, G. Y., & Brooks, D. (2017). Mallacc: Accelerating memory allocation. ACM SIGOPS Operating Systems Review, 51(2), 33-45.

[7] Kuszmaul, B. C. (2015, June). SuperMalloc: a super fast multithreaded malloc for 64-bit machines. In ACM SIGPLAN Notices (Vol. 50, No. 11, pp. 41-55). ACM.
