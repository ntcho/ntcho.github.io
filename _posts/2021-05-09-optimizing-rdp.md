---
title: "Boost Your Remote Desktop Performance"
date: 2021-05-09T13:09:00+09:00
toc: true
toc_sticky: true
header:
  teaser: "https://user-images.githubusercontent.com/13298429/131291498-6240e3af-9567-466d-ae11-024fdd3e1ee8.jpg"
  og_image: "https://user-images.githubusercontent.com/13298429/131291498-6240e3af-9567-466d-ae11-024fdd3e1ee8.jpg"
categories:
  - Software
tags:
  - Windows
  - Remote Desktop Protocol
  - Windows Group Policy
---

Achieving native experience through RDP. Can it reach 60FPS?

# 🧾 Problem

Your RDP environment has poor performance. Poor performance can include the following, but not limited to:

- Frame drops
- Poor image quality or compression
- High latency
- Disabled GPU (e.g. WebGL unavailable)

# 💡 Solution

## 🛠 Materials needed

- A host PC _(Remote Desktop __Session Host__)_
- A client device _(Remote Desktop __Connection Client__)_

### ⚠ Disclaimers

This tutorial is written for Remote Desktop __Connection Client__ in mobile devices such as Android or iOS devices. It might have less performance improvements for clients on PC.

## 📚 Prerequisites

You need a __working Remote Desktop setup__ to configure further. __WOL (Wake-on-LAN) configuration__ for the host PC is recommended since the process requires restarting the host PC.

You would also need administrative rights on your local account in order to change group policy options.

You might need a stable and fast internet connection to see the full performance improvement. Internet connection with speed exceeding 5Mbps is recommended.

### 🖥 Remote Desktop setup

You need __Windows 10 Pro or higher__ in order to connect with Remote Desktop Protocol.

### ⚙ GPU Specifications

You need a GPU or its driver supporting __DirectX 11 or higher__. And to use enhanced graphics on your Remote Desktop session, your system needs to support RemoteFX.

__Note__: Most of modern GPUs and Windows 10 system supports both.
{: .notice--info}

### 📱 Client application

You need a client to establish a connection to the host PC. You can get clients for your platform [here](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients).

## 📑 Problem details

### 😕 Lack of configuration options on mobile clients

![Remote Desktop Connection Window, Experience Tab](https://user-images.githubusercontent.com/13298429/125189747-a1965600-e274-11eb-89d0-9ab812dd5fe3.png)

Compared to performance options in PC client, mobile clients only have the option to change the resolution. We can maximize the performance by modifying settings from the host PC.

### 💔 Lack of hardware acceleration using GPU

![Device Manager Window, Microsoft Remote Display Adapter](https://user-images.githubusercontent.com/13298429/125301110-9e2ec780-e365-11eb-874f-0012bf01e95d.png)

By default, the display adapter driver for Remote Desktop connection is `WDDM`. Therefore, you won't see the GPU installed on your host PC in Device Manager or utilitize its performance. We can disable WDDM graphics on Remote Desktop connection by modifying group policy on the host PC.

## 📇 Step-By-Step Guide

### Step 1. Open _Local Group Policy Editor_

![Run Window, Opening `gpedit.msc`](https://user-images.githubusercontent.com/13298429/125190258-339f5e00-e277-11eb-8a26-a3794617a1c1.png)

Press `Win + R` to open `Run`. Type `gpedit.msc` and press `OK`.

__Note__: You can also search for `Group Policy` or `gpedit` on the start menu.
{: .notice--info}

### Step 2. Configure _Remote Session Environment_ settings

In order to achieve smooth experience, we can compress the image from the host PC to reduce the bandwith, which will result in more frames per second.

__Note__:  I recommend to set all the settings as I did for maximum framerate, but you can fine tune for your use case.
{: .notice--success}

#### Step 2.1. Locate _Remote Session Environment_ folder

![Local Group Policy Editor Window, Remote Session Envionment](https://user-images.githubusercontent.com/13298429/126342772-3c2d134a-7399-46d0-a5b9-4a0f45f4c6f8.png)

Navigate to __Computer Configuration__ – __Administrative Templates__ – __Control Panel__ – __Remote Desktop Services__ – __Remote Desktop Session Host__ – __Remote Session Environment__.

In this folder, you will find all the options you can modify to fine tune your Remote Desktop experience.

__Note__:  The settings in the screenshot above is frame rate optimized settings. You can follow these settings if you are looking for smoothest experience over better image quality.
{: .notice--success}

#### Disable _Use WDDM graphics display driver for Remote Desktop Connections_

{:.no_toc}

![Local Group Policy Editor Window, Use WDDM graphics display driver for Remote Desktop Connections](https://user-images.githubusercontent.com/13298429/125190898-84648600-e27a-11eb-9c90-d798e8cb4520.png)

By disabling this policy setting, Remote Desktop Connections will __not__ use WDDM graphics display driver. In this case, the Remote Desktop Connections will use XDDM graphics display driver.

__⚠ Warning:__ This policy setting change requires restarting the host PC.
{: .notice--warning}

#### Enable _Use hardware graphics adapters for all Remote Desktop Services sessions_

{:.no_toc}

![Local Group Policy Editor Window, Use hardware graphics adapters for all Remote Desktop Services sessions](https://user-images.githubusercontent.com/13298429/125301572-13020180-e366-11eb-8c48-9bcc54c3cc33.png)

By enabling this policy setting, all Remote Desktop Services sessions use the hardware graphics renderer instead of the Microsoft Basic Render Driver as the default adapter. This means you will be able to recognize the GPU installed in the host PC from the Remote Desktop session.

#### Enable _Use advanced RemoteFX graphics for RemoteApp_

{:.no_toc}

![Local Group Policy Editor Window, Use advanced RemoteFX graphics for RemoteApp](https://user-images.githubusercontent.com/13298429/125302089-9a4f7500-e366-11eb-9a59-a72e7cb21c80.png)

By enabling this policy setting, RemoteApp programs will use advanced graphics, including support for transparency, live thumbnails, and seamless application moves.

__Note__: This policy setting applies only to _RemoteApp programs_ and does not apply to remote desktop sessions.
{: .notice--info}

#### Enable _Prioritize H.264/AVC 444 graphics mode for Remote Desktop Connections_

{:.no_toc}

![Prioritize H.264/AVC 444 graphics mode for Remote Desktop Connections](https://user-images.githubusercontent.com/13298429/125302832-45602e80-e367-11eb-86fb-4d18b04a69b0.png)

By enabling this policy setting, the server (host PC) will use H.264/AVC 444 as the codec in an RDP 10 connection where both the client and server can use H.264/AVC 444. This will improve the performance in watching videos encoded in H.264 codec.

#### Enable _Configure H.264/AVC hardware encoding for Remote Desktop Connections_

{:.no_toc}

![Local Group Policy Editor Window, Configure H.264/AVC hardware encoding for Remote Desktop Connections](https://user-images.githubusercontent.com/13298429/125303610-e353f900-e367-11eb-97e5-c3820901c869.png)

By enabling this policy setting, Remote Desktop Conection will try to use H.264/AVC hardware encoding support for applicable clients. This will improve the performance in watching videos encoded in H.264 codec if the client supports hardware encoding.

#### Enable and configure _Configure compression for RemoteFX data_

{:.no_toc}

![Local Group Policy Editor Window, Configure compression for RemoteFX data](https://user-images.githubusercontent.com/13298429/125304654-b9e79d00-e368-11eb-874c-ee71cb261604.png)

By enabling this policy setting, you can specify which RDP compression algorithm to use.

I prioritized frame rates over image quality, so I selected __Optimized to use less network bandwidth__. This will decrease the network bandwith usage with compressing the image a bit more. From my experience, the image compression wasn't noticeable but it gave me higher frame rates. I recommend using this option.

If you need better image quality even with lags, you can select __Optimized to use less memory__ or __Do not use an RDP compression algorithm__ option.

#### Enable and configure _Configure image quality for RemoteFX Adaptive Graphics_

{:.no_toc}

![Local Group Policy Editor Window, Configure image quality for RemoteFX Adaptive Graphics](https://user-images.githubusercontent.com/13298429/125306522-421a7200-e36a-11eb-8e58-7767d3a67f54.png)

By enabling this policy setting and set quality to __Low__, RemoteFX Adaptive Graphics uses an encoding mechanism that results in low quality images. This mode consumes the lowest amount of network bandwidth of the quality modes. This will increase the maximum refresh rate of the Remote Desktop session.

If you need better image quality even with lags, you can select __Medium__ or __High__ option. The performance will depend on your internet connection.

#### Enable _Enable RemoteFX encoding for RemoteFX clients designed for Windows Server 2008 R2 SP1_

{:.no_toc}

![Local Group Policy Editor Window, Enable RemoteFX encoding for RemoteFX clients designed for Windows Server 2008 R2 SP1](https://user-images.githubusercontent.com/13298429/126171939-cdfda2aa-6f86-439a-a5af-e8a8d5626161.png)

By enabling this policy setting, you can configure graphics encoding to use the RemoteFX Codec on the Remote Desktop Session Host server. This policy setting applies only to clients that are using Remote Desktop Protocol (RDP) 7.1, and does not affect clients that are using other RDP versions. This will enhance the Remote Desktop experience in certain client version.

#### Enable and configure _Configure RemoteFX Adaptive Graphics_

{:.no_toc}

![Local Group Policy Editor Window, Configure RemoteFX Adaptive Graphics](https://user-images.githubusercontent.com/13298429/126173697-411fac1f-fb19-467b-a7d6-423375118768.png)

By enabling this policy setting, you can configure the default RemoteFX experience settings.

As mentioned above, I prioritized frame rates over image quality, so I selected __Optimize for minimum bandwidth usage__. This will decrease the network bandwith usage with compressing the image. From my experience, the image compression wasn't noticeable for most of the time but it gave me higher frame rates. I recommend using this option.

#### Disable _Use WDDM graphics display driver for Remote Desktop Connections_

{:.no_toc}

![Local Group Policy Editor Window, Use WDDM graphics display driver for Remote Desktop Connections](https://user-images.githubusercontent.com/13298429/126334434-3f62b0c1-3b1a-4e57-8666-226adcbfebee.png)

By disabling this policy setting, Remote Desktop Connections will ***not*** use WDDM graphics display driver. In this case, the Remote Desktop Connections will use XDDM graphics display driver. This will enable you to utilitize the performance of your GPU installed on the host PC.

__⚠ Warning:__ This policy setting change requires restarting the host PC.
{: .notice--warning}

### Step 3. Configure _RemoteFX_ settings

In order to utilitize the GPU installed on the host PC, we have to enable __RemoteFX__ on the host PC first.

#### What is RemoteFX?

__Microsoft RemoteFX__ is a set of technologies that enhance visual experiences in Remote Desktop Protocol (RDP). It utilitizes the GPU installed on the host PC to render the graphics and compress it over the network to optimize the bandwith usage. It means it uses more of your hardware to make your Remote Desktop environment smoother.

For example, if you are trying to send 1080p 60fps session over the internet without any compression, you would need almost 3 Gbit/s network connection. But with RemoteFX configured, the host PC uses an efficient compression algorithm to reduce the bandwith usage, and also processes it with the installed hardware. And also, since the graphics are rendered from the server, you can use graphic intense applications over Remote Desktop.

Read more: [RemoteFX on Wikipedia](https://en.wikipedia.org/wiki/RemoteFX)
{: .notice}

#### Step 3.1. Locate _RemoteFX for Windows Server 2008 R2_ folder

![Local Group Policy Editor Window, RemoteFX for Windows Server 2008 R2](https://user-images.githubusercontent.com/13298429/126506569-c5265f65-e998-4131-a8ee-02b87c84c0e4.png)

Navigate to __Computer Configuration__ – __Administrative Templates__ – __Control Panel__ – __Remote Desktop Services__ – __Remote Desktop Session Host__ – __Remote Session Environment__ - __RemoteFX for Windows Server 2008 R2__.

In this folder, you will find all the options you can modify to fine tune your RemoteFX configurations.

#### Enable _Configure RemoteFX_

{:.no_toc}

![Local Group Policy Editor Window, Configure RemoteFX](https://user-images.githubusercontent.com/13298429/126499389-e64560f1-c0bc-40db-a518-c2c64b3dc4fa.png)

By enabling this policy setting, RemoteFX will be enabled. RemoteFX will deliver a rich user experience over LAN connections and RDP 7.1.

#### Enable and configure _Optimize visual experience when using RemoteFX_

{:.no_toc}

![Local Group Policy Editor Window, Optimize visual experience when using RemoteFX](https://user-images.githubusercontent.com/13298429/126499724-67935e89-2a37-41a7-96d8-3686c74abe6b.png)

By enabling this policy setting, you can specify the visual experience in Remote Desktop Connection (RDC) connections that use RemoteFX.

For __Screen capture rate (frames per second)__, I selected __Highest (best quality)__ for maximum refresh rate, and for __Screen Image Quality__, I selected __Medium (default)__ for decent image quality. In my case, selecting __Lowest__ resulted in pixelated graphics in some occasions.

#### Enable and configure _Optimize visual experience for Remote Desktop Service Sessions_

{:.no_toc}

![Local Group Policy Editor Window, Optimize visual experience for Remote Desktop Service Sessions](https://user-images.githubusercontent.com/13298429/126500730-9762007d-3f05-4043-9385-71902714dfc8.png)

By enabling this policy setting, you can specify the visual experience that remote users receive in Remote Desktop Services sessions.

In my case, I watched some videos and photos through the Remote Desktop, so I selected __Rich multimedia__. You can configure this option to your preferences.

### Step 4. Apply changes

After making changes in __Local Group Policy Editor__, you can just close the window as the settings are automatically applied. If you changed any settings that require restart, restart your host PC once. After your host PC restarts, you can reconnect to the session and compare the differences in refresh rate or image quality.

#### Bonus: Testing your refresh rate

{:.no_toc}

If you are not sure about whether you are getting 60fps on your Remote Desktop session, you can use [testufo.com](https://testufo.com/) to test your refresh rate.

#### Troubleshooting: Still having performance issues?

The problems could be one or more of the following:

- Internet connection speed / latency between client and host
- Driver problems for GPU installed on host PC
- Didn't restart the host PC after configuration

🚩 Disclaimer: This post was inspired from [this post by _ceol_](https://m.blog.naver.com/qhagosk0277/221957885625).
{: .notice}