---
title: "Modifying an APK File"
date: 2021-04-30T20:50:00-04:00
toc: true
toc_sticky: true
categories:
  - Software
tags:
  - Android
  - Reverse Engineering
  - APK
---

Decompiling and recompiling an APK can be easily done using __Apktool__.

# 🧾 Problem

You have an `.apk` file that you want to __view/edit__ these following contents:

- Resources such as `strings.xml` or other drawables
- App configurations such as `AndroidManifest.xml`
- Source codes such as __Java classes__

This post is not intended for piracy and other non-legal uses. This post could be used for localizing, adding features, supporting custom platforms, analyzing applications.
{: .notice--danger}

# 💡 Solution

## 🛠 Materials needed

`.apk` file that you want to decompile.

#### Don't have the `.apk` file?

If the app is listed on the Google Play Store, you can use websites such as [apkpure.com](https://m.apkpure.com), [apkmirror.com](https://www.apkmirror.com).

If it's not listed but installed on your Android device, you can use APK extracting apps such as [Apk Extractor](https://play.google.com/store/apps/details?id=com.ext.ui&hl=en) to get the `.apk` file for the app.

Warning: APK decompiliation using Apktool might not work properly for apps encrypted with [ProGuard](https://developer.android.com/studio/build/shrink-code#enable) or other methods.
{: .notice--warning}

## 📚 Prerequisites

You would need basic knowledge of using the Android SDK and how an Android app is built.

### Java JRE/JDK

At least __Java 1.8__ should be installed to use Apktool. If you intend to rebuild the app, you would need __JDK__ to use the command `keytool` and `jarsigner`.

To check your Java version, run `java -version` on command prompt. To install Java, click [here](https://java.com/download/).
{: .notice}

To install JDK, click [here](https://www.oracle.com/java/technologies/javase-downloads.html).
{: .notice}

### [Apktool](https://ibotpeaches.github.io/Apktool/)

Click [here](https://ibotpeaches.github.io/Apktool/install/) to learn how to install __Apktool__ for Windows, Linux and macOS.

If you want to use the command `apktool` globally, you can install Apktool on `C:/Windows`. But if you plan to only use it for a short time, you can install Apktool on your project folder to only use it in the folder.
{: .notice}

#### [Apktool Online](http://www.javadecompilers.com/apktool) _(optional)_

If you only intend to decompile the app without rebuilding the app to the `.apk` format, you can use [Apktool online](http://www.javadecompilers.com/apktool) without installing Apktool on your computer.

If you intend to build the app with the changes you made, you need to install Apktool.

### [APK Decompiler](http://www.javadecompilers.com/apk) _(optional)_

If you intend to analyze the source codes such as Java classes, you need to use [dex2jar compiler](https://sourceforge.net/projects/dex2jar/files/). You can use it online from [here](http://www.javadecompilers.com/apk).

__Tip__: I recommend using both __Apktool__ and __APK decompiler__ in order to trace which resource is used where and how.
{: .notice--info}

## 📇 Step-By-Step Guide

### Step 1. Preparing project folder

Put your `.apk` file in a project folder of your choice. In this tutorial, the project folder is named as `\proj`.

Your project folder would look like this:

```
proj
+-- app.apk     // the app to decompile
+-- apktool.bat
+-- apktool.jar
```

In this tutorial, I installed the Apktool locally in the project folder. Therefore `apktool.bat` and `apktool.jar` exists in the same folder.
{: .notice}

### Step 2. Decompiling the `.apk` file

Decompile `app.apk` using Apktool.

```ps
.\apktool.bat d app.apk
```

In this tutorial, I used Windows PowerShell for command prompt.
{: .notice}

After execution, you will see the `\app` folder created by Apktool. In the folder, you can see all the resources such as `AndroidManifest.xml` and the resource directory `\res` and its contents in original form.

If you want to edit `AndroidManifest.xml` or the resource, you can edit and overwrite to the original file.

#### Decompiling with readable source code

Source code is decompiled to `.smali` files, making it unreadable. So if you want to read the source code, you have to use the [APK Decompiler](http://www.javadecompilers.com/apk) using dex2jar compiler.

Open [APK Decompiler](http://www.javadecompilers.com/apk) on your browser, and choose your `.apk` file with the ***Choose File*** option. Decompile the file by clicking on ***Upload and Decompile*** button.

After the processing is done, download the results by clicking on ***Save*** button. Download the `.zip` file to the project folder and unzip it.

### Step 3. Recompiling the app

Recompile `\app` into `app.apk` using Apktool.

```ps
.\apktool.bat b app
```

The built APK would be located in `\app\dist`. The changes you made in the `\app` folder is applied to the new APK file.

#### Step 3.1. Creating a key for signing

To install the edited APK to an Android device, you need to sign it with a signature key. Create a key using `keytool`, a tool provided as a part of JDK.

```ps
keytool -genkey -v -keystore release.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000
```

Enter password, name, organization information, address for the new keystore. This will generate `release.keystore`, and this will be used to sign your new app.

The password will be used as a passphrase to sign the app.
{: .notice--warning}

#### Step 3.2. Signing the app

Sign the APK with the generated key using `jarsigner`, a tool provided as a part of JDK.

```ps
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore release.keystore app/dist/app.apk alias_name
```

Enter the passphrase for your keystore. This will sign and update the apk file `\app\dist\app.apk`.

### Step 4. Installing the app

Locate and install the apk file `\app\dist\app.apk` to your device of choice.

If you don't see changes after installing the app, double check whether you installed the updated version in `\dist`, not the original APK file. 
{: .notice--info}