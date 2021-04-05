---
title: "ReFlex Review - ASPLOS '17 "
date: 2018-12-02T07:31:37Z
# weight: 1
# aliases: ["/first"]
tags: ["flash", "QoS", "dpdk", "disaggregation", "asplos"]
categories: ["paper-review"]
author: "Me"
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
searchHidden: true
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

The Paper[1] presents a remote-flash-accessing system called ReFlex, which provides comparable performance to accessing local Flash. The authors point out that remote access to hard disks and remote access to Flash using RDMA (Remote Direct Memory Access) are two common ways to offer flexibility and efficiency in a data center. However, they prove these existing approaches are facing two critical problems: the first is how to tradeoff between performance and cost, while the second is how to provide predictable performance without interferences. Thus they design this system to avoid the problems current systems may raise while still meeting the performance expectations.

ReFlex uses a tightly integrated data plane architecture to provide low latency and high throughput access to remote Flash. In the first place, the authors exploit the virtualization features in the hardware, though it is a software system, to transfer data and requests without copying them. Also, the polling-based execution model eliminates the interruptions of process. Thanks to the novel QoS scheduler, ReFlex serves different types of tenants to enforce SLOs (Service-Level Objectives), even for different data streams. By these means, the authors state that ReFlex is able to deal with thousands of tenants and network connections due to its high utilization to CPU cores.

The architecture of ReFlex is Client-Server model: server is responsible for executing the models mentioned above and clients are written for applications to access the servers. Moreover, the authors re-design the control plane that runs on every ReFlex server to balance the loads. ReFlex outreaches its competitors on tail latency, request/response throughput and CPU resources utilization, with well-behaved QoS scheduling and tenant isolations. Therefore, ReFlex can serve up to 850K IOPS per core over TCP/IP networking, with merely 21us addition over direct access to local Flash.

Reference:

[1] Klimovic, A., Litz, H., & Kozyrakis, C. (2017). Reflex: remote flash ≈ local flash. ACM SIGOPS Operating Systems Review, 51(2), 345-359.
