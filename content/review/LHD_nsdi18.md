---
title: "LHD Review - NSDI '18"
date: 2018-12-02T07:31:37Z
# weight: 1
# aliases: ["/first"]
tags: ["caching", "key-value", "NSDI"]
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

Beckmann, N., Chen, H., & Cidon, A. (2018, April). LHD: Improving Cache Hit Rate by Maximizing Hit Density. In 15th {USENIX} Symposium on Networked Systems Design and Implementation ({NSDI} 18). USENIX} Association.

Least hit density (LHD) [1] is a brand-new cache replacement algorithm designed for key-value caches. An improvement to the cache hit rate of current distributed, in-memory key-value caches has shown more and more importance in demand of latency sensitive applications recently. However, some most popular used policies like least recently used (LRU) or first in first out (FIFO) have their own problems. For instance, The performance of LRU would be largely lowered down if cache pollution happens. Other complicated policies that introduce parameter tuning may cause unreliable performance and some mechanisms employ synchronized states which could increase the cost and decrease the throughput.

However, the strategy for specific workload can be somehow improved. Beckmann, N., Chen, H., & Cidon, A. propose the LHD which would predict the expected hit-per-space (hit density) that is consumed by each object. The main idea of LHD is to monitor the probability of an object that would hit the cache during the lifetime and divide it by the resources it would consume. The basic evicting step, just like most of the algorithms in this area, is replacing the cache block that has the lowest hit density. In other words, the insight is to make small objects that hit frequently. The hit probability can be calculated by this simple formula: collected hits / (size * lifetime). What’s more, LHD would absorb the extra information about objects intuitively. 

In order to prove the effectiveness of this algorithm, they implement a specific Memcached-based application named RankCache which includes the LHD ranking function. Besides hit density, RankCache can also support other arbitrary ranking functions. For management of memory, RankCache chooses to use slab allocation in order to make the performance of insertion and eviction is predictable. As a approximate scheme, RankCache has a high tolerance to loosely synchronized events and the aging information collecting from Memcached, and its allocator was succeeded to enforce delicate synchronizations. 

According to the data shown by the authors of LHD, RankCache would have much higher throughput than its competitors under specific hit ratio. After introducing information on tags, RankCache performs better. However, the eviction performance is not good in terms of SET operations. This performance gap may be somehow alleviated by the performance gain from the GET operations, which are much more time-consuming in most of the key-value cache systems.

### Reference

[1] Beckmann, N., Chen, H., & Cidon, A. (2018, April). LHD: Improving Cache Hit Rate by Maximizing Hit Density. In 15th {USENIX} Symposium on Networked Systems Design and Implementation ({NSDI} 18). USENIX} Association.
