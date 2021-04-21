---
title: "Mach Review"
date: 2020-01-13T07:31:37Z
# weight: 1
# aliases: ["/first"]
tags: ["kernel", "operating-system", "IPC", "multi-processor"]
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
Mach is a new kernel design based on UNIX that supports multi-processors by using inter-process communications (IPC). Mach presents several new primitive abstractions including task, thread, port, and message. Task could be a multi-threaded execution environments that contains all the resources including the virtual memory spaces and protected access to ports, while threads are the smallest computation. What's more, Mach manages virtual memory with copy-on-write and read/write sharing ability. It splits the machine-dependent and independent sections by using data structures like address maps, share maps, VM objects and page structures. Also, they designed the IPC for Mach by sending and receiving messages through queue-like protected ports.

### Significance
Mach is the first influential system that supports secure inter-process communications. It incentives a lot of following works on micro-kernels. Its design has been incorporated in many famous systems like OSFMK, NeXT, Mac OS and its implementations was even used as the core of Mac OS, so as to the iOS, iPadOS, watchOS and tvOS.

### Questions

* How will a receiver know a message is coming? By polling or interrupting?
* As for the support to the IPC over the network, local ports are mapped into the network ports at both servers and clients. Who is responsible to maintain these mappings and how would servers and clients negotiate a consistent one?
* How would the system cope with small IPC messages and large IPC messages? Batching? Alignment?

### Reference

[1] Accetta, M., Baron, R., Bolosky, W., Golub, D., Rashid, R., Tevanian, A., & Young, M. (1986). Mach: A new kernel foundation for UNIX development.