---
layout: post
title: Benchmarking Static Sites
---

This is a post on benchmarking static websites using s3, githubpages, and nginx running on ubuntu.

In this article I will be benchmarking these sites based on time to load, not server response times. To do this I will be using phantomjs to capture document ready calls which will give the true time to load.

This benchmark will be setup on a series of Digital Ocean droplets. It will then be benchmarking services located on aws. This is to mimic real world scenarios.

Each benchmarker will be running in their own container preconfigured with phantomjs 2.0. Orchestration was configure using docker-machine.

The steps will be listed below

docker-machine create \
    --driver digitalocean \
    --digitalocean-access-token $DO_TOKEN githubpages

I then have native docker access to a bunch of different hosts as if they are running natively on my computer.


In this post I will be benchmarking different static site implementations. The services being benchmarked are AWS S3, Github Pages, and nginx running on ubuntu.

The benchmarks are structured out into two rounds. The first being the default of the service; no optimizations were taken in deploying the static site. It's 100% the stock. Round two takes into account for the optimzations, files are minified, gzipped, and compressed.

Benchmarks are being recorded using phantomjs 2.0.0. Instead of running benchmarks using something like ab or jmeter, I decided to use something that would mimic a user using their browser.
