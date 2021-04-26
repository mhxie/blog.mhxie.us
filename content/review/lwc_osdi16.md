---
title: "lwC Review- OSDI '16"
date: 2020-01-17T07:31:37Z
# weight: 1
# aliases: ["/first"]
tags: ["isolation", "threading", "userspace", "OSDI"]
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


### Summary
This paper introduces the new user-level thread implementation called light-weight context (lwC). They utilized the vmspace structure in FreeBSD to create separate memory spaces for new lwC entities. They further reduced the TLB flush overhead by cleverly enabling the "process context identifier" (PCID). To provide fine-grained resource sharing across lwC entities, parent lwC would assign access capabilities to child lwC by giving file descriptor that specify the range and rights of sharing resources. 
lwCs could be effectively leveraged to isolate sessions in web servers. They showed the performance was only compromised a little by introducing lwC but with better isolation capability. lwCs also provide efficient isolation to sensitive security information in cryptographic libraries implementations. This paper also presented how snapshot and rollback could be easily achieved to minimize the overhead every time a new session is created.


### Significance
Unlike the previous coroutine-style implementations, lwC provide more fine-grained, privilege-separated and modern context switching implementation in user space. It is a new efficient and practical solution to achieve better user-space isolation within a process.

### Questions

1. What is the OS's sandboxing mechanism, why does it force a child lwC to switch back to parent lwC when invoking a system call with LWC_SYSTRAP?
	Because of safety problem, avoid returning to an address right before syscall;
2. lwCs are not schedulable entities, Why don't they provide schedule options like returning scheduled operations?
3. How much memory overhead will be added per lwC? Would it largely affect the number of concurrent sessions when applying it in production web servers?
	CoW, a lot reading; or writing a lot but not modifying data; changing pointer; PCID;


### Reference

[1] Litton, J., Vahldiek-Oberwagner, A., Elnikety, E., Garg, D., Bhattacharjee, B., & Druschel, P. (2016). Light-weight contexts: An {OS} abstraction for safety and performance. In 12th {USENIX} Symposium on Operating Systems Design and Implementation ({OSDI} 16) (pp. 49-64).