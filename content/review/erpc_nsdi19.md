---
title: "eRPC Review - NSDI '19 "
date: 2019-04-13T07:35:40Z
# weight: 1
# aliases: ["/first"]
tags: ["rpc", "dpdk", "nsdi"]
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
editPost:
    URL: "https://github.com/mhxie/blog.mhxie.me/blob/main/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

BDP(Bandwidth delay product): bandwidth-delay product (BDP's) worth of buffer, where bandwidth is the bottleneck link and delay is the round-trip time (RTT) between sender and destination.

Optimization more on the sender part?

### Client-driven protocol

eRPC is a datacenter remote procedure call (RPC) framework or say library that provides high performance RPC based on either lossy ethernet or lossless fabrics.

Based on the high speed packet IO framework DPDK, the main design of eRPC lies on the following optimizations:

(1) Zero-copy request processing: assuming the common case of lossless network, in client end, eRPC will not allow a TX queue with a reference to the request msgbuf when that request is being processed, which means there should not be any retransmitted packet in their assumption.

(2) Preallocated responses: following (1), in server end, eRPC does not need to copy data from RX buffer ring to dynamically allocated application buffer called msgbuf but only using DMAed data whose headers are stripped in advance.

(3) Multi-packet RQ: they came up with a BDP flow control that limit the number of in-flight packets based on the allowable BDP/packet size (e.g. MTU)

### Common cases optimizations:

(4) Rate limiter bypass: it will directly place the data to rx/tx buffer in userspace rather than rate limiter.

(5) Congestion control (timely) bypass: based on the report showing that data centers are usually underutilized and uncongested, they intentionally omit the congestion control until

(6) Batched RTT time stamp: timestamping overhead can hurt a lot because each call to rdtsc() cost 8ns when it comes to a scale of million-level. So they batched this call by sampling.

So basically, eRPC is a composition of optimization ideas with the assumption of lossless and uncongested situation. As a result, eRPC achieves up to 10 mpps for small RPC request or 75Gbps for large messages by using a single core. What's more, the replication latency is as low as 5.5us which is even better than programmable hardware options.

### Future directions:

Statistical multiplexing for session credits

### Drawbacks:

- eRPC is pretty much optimized for small RPC request, the throughput result is based on a packet as large as 8MB which is a ridiculous size in most of cases.
- This paper does not show any diagram of the performance what if the common case is not satisfied.
- They do not implement SACKs due to the engineering complexity.
- Bypassing the rate limiter is wired and inconvenient in terms of performance isolation and it is by no means a good choise from the perspective of virtualization.
