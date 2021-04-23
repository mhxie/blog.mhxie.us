---
title: "Review for Digital Signature for Flows and Multicasts - ToN'99"
date: 2019-01-22T07:31:37Z
# weight: 1
# aliases: ["/first"]
tags: ["signature", "multicast", "verification", "ton"]
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
Digital Signature for Flows and Multicasts [1]
 
Data signing and verifying is a pretty time-consuming operation pair. This problem becomes severe when applying it to the encryption of network data flows. A standard solution to leasing such laborious work is to use the digest of flows which contains several or hundreds of packets in a row. However, the intrinsics of best-effort delivery in current Internet make this solution impractical. It is even more challenging to get almost in-order and entire sequences of packets of a multicast flow. If we lost some of the packets in a flow, the decryption or verification would be blocked by them, and these packets may never be able to receive in multicast flows.
 
Chung et al. proposed two chaining techniques as opposed to the current sign-each approach. The general purpose of these two approaches is to make packets in a flow verifiable individually. The first one is called star-chaining: each packet holds its position in the flow, the digest of the rest of the packets in the block it belongs to, and the digest of the digests of all packets in the block. For the first packet, the receiver could verify it by recomputing the combination of the digest of the payload and the digests of the others and compare it to the block digest. After the first verification, the verifier only needs to compare if its digest of the payload is the same as the first packet. They improved this method to so-called tree-chaining by using less space (log of block size) within a packet but not increasing the computation load a lot (in case that computation speed scale linearly with the packet length) because what the overhead for a packet is decreased to the sibling and parents of itself in the tree built from the block.
 
Based on tree chaining with degree-two (binary), they further perfected the performance of signature operations by extending the FFS signature scheme on several aspects:

* They used the first satisfiable prime numbers to reduce the verification time and critical size.
* They employed the Chinese Remainder Theorem to simplify it more. What is more, the authors pointed out the duplicate part of the computation, so they just precomputed those in advance and stored it for future use.
* They provided adjustable and incremental verification that allows a more flexible tradeoff between security level and computation time.
 
In general, this paper provided many good insights that deserve attention. However, there are still some spaces to improve since their assumption may not apply to all possible situations (e.g., high-speed networking). By removing some redundancy and build more paradigms, the verification pair could speed up more in the future.

### Reference

[1] Wong, C. K., & Lam, S. S. (1998, October). Digital signatures for flows and multicasts. In Proceedings Sixth International Conference on Network Protocols (Cat. No. 98TB100256) (pp. 198-209). IEEE.