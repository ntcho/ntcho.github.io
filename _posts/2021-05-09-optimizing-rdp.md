---
title: "Optimizing RDP Environment for Windows"
date: 2021-05-09T13:09:00+09:00
categories:
  - Software
tags:
  - Windows
  - Remote Desktop Protocol
  - RDP
  - Optimization
  - Windows Group Policy
---

Achieving native experience through RDP. Can it reach 60FPS?

# Problem

Your RDP environment has poor performance. Poor performance can include the following, but not limited to:

- Frame drops
- Poor image quality or compression
- High latency

# Solution

## Materials needed

- A host PC _(Remote Desktop Session Host)_
- A client PC _(Remote Desktop Connection Client)_


## Prerequisites

You would need basic knowledge of using the Android SDK and how an Android app is built.

### Java JRE/JDK

At least __Java 1.8__ should be installed to use Apktool. If you intend to rebuild the app, you would need __JDK__ to use the command `keytool` and `jarsigner`.

> To check your Java version, run `java -version` on command prompt. To install Java, click [here](https://java.com/download/).

> To install JDK, click [here](https://www.oracle.com/java/technologies/javase-downloads.html).


## Step-By-Step Guide

### Step 1. Preparing project folder
