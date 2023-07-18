---
title: Running singularity containers in a High Performance Computing (HPC) environment
author: Jiayi
date: '2023-07-18'
slug:
categories:
  - Programming
tags:
  - container
  - singularity
  - HPC
  - bioinformatics
---

## Preface
Working in an HPC environment, the ["dependency hell"](https://en.wikipedia.org/wiki/Dependency_hell#:~:text=The%20dependency%20issue%20arises%20when,versions%20of%20the%20shared%20packages.) is the most bothersome nuisance. (Yep, I personally regard it "the most", not "one of the most".) 

For instance, in R, users often receive message such as: 

```r
"Error: package or namespace load failed for XXX:package YYY was installed before R 4.0.0: please re-install it"
``````

The solution could be relatively easy, you may upgrade to the latest version of R or trying re-installing the YYY package. 

In Linux, however, things could be much more painful.   
![](/img/linux-dependencies-error.png)  
Especially when working in an HPC, it is nearly impossible to install/update pacakges that requires sudo privilege.   

Luckily, [my ex-boss, Dr. Chang](https://www.linkedin.com/in/jeffrey-chang-4676192), introduced me the concept of app containerization using tools such as [Docker](https://www.docker.com/) and [Singularity](https://sylabs.io/).  I tried, and realized that I cannot live without them. They are truely my life savers.   

In this post, I would like to provide a step by step tutorial on how to run singularity containers in an HPC environment.   

### Table of Contents
1. [What is a container?](#what-is-a-container)
2. [Where can I get containers](#where-can-i-get-containers)  
  2.1 [Public resources](#public-resources)  
  2.2 [Build my own container](#build-your-own-container)
4. [Fourth Example](#fourth-examplehttpwwwfourthexamplecom)


## What is a container? 
To begin with, a disclamer: I'm not a CS person. That means I won't be using terminologies. Instead I'll use just plain words. 

Think of this scenario: you got a message when running a software X on your HPC: 

> The software X is update, however, the software Y is not. Since software X depends on some functions wrapped in Y, so you are not able to run X now.  

-- bang, ERROR.   

You might think, ok, let me update Y.  You tried, and got another message:  

> You are not allowed to update Y since you are not the super user of this computer.    

-- bang, ERROR again.   

This is the simplest example of dependencies hell.  In reality, it could be much more complex since the software dependency manner is not one-to-one; sometimes you may need to install more softwares that requires "sudo" privilege.   

One way of solving this is to build your own Utopia, where you take fully control:   
1. You can keep your softwares at your favourite version, and they don't auto-update themselves.  
2. Your softwares has the appropriate dependency softwares, and they always work well with each other.   
3. Your are the super user of this Utopia, so you can install anything that requires sudo privilege.   
4. You can still read and write your data (files, images, etc.) to the outside world.  

Another advantage of this Utopia is that you can reproduce your analyses -- the package version are controlled, so just run your scripts and it'll always give you the same results -- nothing's gonna change.  

This kind of Utopia world is the so called "container".  

As a bioinfo hobbyist and a singularity lover, I'll be using the bioinformatics related tools to illustrate how to build and use singularity containers. As for the other containerization tools, you can simply check their documentations. 

## Where can I get containers? 
There are two ways of acquiring a container, downloading one from the public, or build one from scratch:   

### Public resources  


### Build my own container  


