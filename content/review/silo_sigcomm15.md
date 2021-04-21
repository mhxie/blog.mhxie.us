---
title: "Silo Review - SIGCOMM '15"
date: 2021-04-16T23:07:37Z
# weight: 1
# aliases: ["/first"]
tags: ["cloud", "SLO", "tail-latency", "pacing", "VM", "SIGCOMM"]
categories: ["paper-review"]
author: "Original"
# author: ["Me", "You"] # multiple authors
showToc: false
TocOpen: false
draft: true
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

This paper [1] presents Silo, a system that can gurantee both latency and
throughput service level objectives (SLOs) in multi-tenant data centers.

### Design Goals

1. guaranteed bandwidth;
2. guaranteed packet delay;
3. guaranteed burst allowance;

### Challenges

1. > delay is an additive end-to-end property
2. queueing delay is the marjor factor along the critical path and it is hard to
   precisely enforce indepedent flows on all networking knobs
3. > guaranteed packet delay is at odds with the guaranteed burst requirement

<!-- ### Problem Statement -->

### Novelty

- network-calculus-based VM placement:
- hypervisor-based policing: use "void" packets to be batched with actual
  packets that allows pacer to space out actual data packets without hardware
  supports

### Advantages

- no need to change the underlyng hardware infrastructures;

### Disadvantages

- worst-case assumption decreases the server utilizations in non-bursty scenario;

### Reference

[1] Jang, K., Sherry, J., Ballani, H., & Moncaster, T. (2015, August). Silo: Predictable message latency in the cloud. In Proceedings of the 2015 ACM Conference on Special Interest Group on Data Communication (pp. 435-448).
