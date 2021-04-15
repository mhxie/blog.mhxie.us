---
title: "Crail Review - IEEE Data Eng. Bull. Vol 40"
date: 2018-12-02T07:31:37Z
# weight: 1
# aliases: ["/first"]
tags: ["disaggregation", "RDMA", "userspace", "file-system"]
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

The performance of networking and storage hardware has developed rapidly in recent years. Emerging new interfaces like RDMA and NVMe make user-level hardware access and asynchronous I/O possible, allowing upper applications to take full advantages of these high-performance hardware. However, the authors of Crail (Stuedi, P. et al., 2017) point out that most of the recent applications are too low-brewed and too focused on serving particular workloads to satisfy general task requirement.

Crail is a user-level I/O architecture for Spark, an Apache data processing systems, fully utilising the high-performance networking and storage hardware from scratch. Usually, the bottom of a stack is where hardware integrations happen. This problem is also known as Deep Layering, one of the software-related I/O bottlenecks (Animesh Trivedi et al., 2016). In addition, Data Locality and Legacy Interfaces contribute to the difficulties of leveraging high-performance hardware in Spark and its analogs. The authors state that Crail has solved the problems listed above by using storage tiering and resource disaggregation over a high-performance RDMA network.

The targeting I/O operations that Crail focuses on is the temporary data, which was generated from data shuffling, broadcasting within jobs or shared across jobs. In fact, two of the most frequently used workloads supported by Spark would gain remarkable efficiency improvement by adopting the Crail architecture.

The authors establish that the actual overheads in Spark network operations are caused mainly by the software stack rather than networking hardware. They make a consequent conclusion, proved by their solid experiments, that overheads of running these engines could be reduced by the disaggregated environment. This finding further supports the research on flash storage disaggregation (Klimovic, A. et al., 2016), making lots of relevant thoughts explorable.

### Reference

[1] Stuedi, P., Trivedi, A., Pfefferle, J., Stoica, R., Metzler, B., Ioannou, N., & Koltsidas, I. (2017). Crail: A High-Performance I/O Architecture for Distributed Data Processing.Crail: A High-Performance I/O Architecture for Distributed Data Processing. IEEE Data Eng. Bull., 40(1), 38-49.

[2] Klimovic, A., Litz, H., & Kozyrakis, C. (2017). Reflex: remote flash ≈ local flash. ACM SIGOPS Operating Systems Review, 51(2), 345-359.