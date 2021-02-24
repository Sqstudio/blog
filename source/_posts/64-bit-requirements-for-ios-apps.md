---
title: 64-bit requirements for iOS apps
tags: []
id: '1041'
categories:
  - - 移动探索
date: 2015-04-14 11:07:52
---

本文系转载：http://easynativeextensions.com/making-your-ios-apps-universal/   At the end of last year Apple announced their new requirements for submitting apps to the app store:

*   from **February 1st 2015** all [new apps must include 64-bit support](https://developer.apple.com/news/?id=10202014a);
*   from **June 1st 2015** all [app updates will also have to support 64 bits](https://developer.apple.com/news/?id=12172014b).

This doesn’t pose much of a problem for native apps, but we AIR developers were stranded… Until last week, when [Adobe released AIR 16 beta in Adobe Labs](http://labs.adobe.com/downloads/air.html). Let us go through the steps necessary for rebuilding your apps and ANEs to support 64 bits.

## Step 1: Update your AIR SDK

**1.1.** Download AIR SDK 16 beta from Adobe Labs: [http://labs.adobe.com/downloads/air.html](http://labs.adobe.com/downloads/air.html)

**Note:** if you use Flash Builder and/or the Flex SDK, you need the last download link, **AIR 16 SDK for Flex Developers**.

**1.2.** Make a backup copy of your AIR SDK folder. **1.3.** Overlay the new SDK over the old one: **1.3.1** If you are a **Windows** user, unzip the download and copy the contents over your AIR SDK. **1.3.2.** On **Mac** you can do the same and ask Finder to merge folders or, you can copy the archive in the root of your AIR SDK and run the following command in the Terminal:

1

sudo tar jxvf air16\_sdk\_sa\_mac.tbz2

## Step 2: Update your app descriptor

If you are a veteran AIR developer, you are used to this, but here it is just in case. Open your app descriptor file – usually named **your-app-name-app.xml** and found in your project’s **src/** folder and make sure the namespace at the top of the file points to **16.0**:

1

<application xmlns\="http://ns.adobe.com/air/application/16.0"\>

## Step 3: Rebuild your iOS ANEs

Any native extensions for iOS that your app uses will also need to be built with 64-bit support. If you have the source code for these ANEs, this is what you do: **3.1.** Make sure you are using Xcode 6 and iOS SDK 8. **3.2.** In your **Xcode** project, go to **Build Settings** > **Architectures** and first make sure that **Architectures**is set to **Standard architectures (armv7, arm64),** then set **Build Active Architecture Only** to**No**. Without the last change the compiler will default to building a binary that supports only one architecture, which matches the device that you have connected at the moment or the simulator version you have made active – good idea for a debug build, but not for release. [![Xcode 64bit architecture setting](http://easynativeextensions.com/wp-content/uploads/2015/01/Xcode-64bit-architecture-setting.png)](http://easynativeextensions.com/wp-content/uploads/2015/01/Xcode-64bit-architecture-setting.png)   Now make a clean build of your ANE.

Here are a couple of resources that will help you automate your ANE building and packaging:

*   [Automatic ANE packaging](http://easynativeextensions.com/automatic-ane-packaging/)
*   [Packaging your ANE in Flash Buidler](http://easynativeextensions.com/package-your-ane-in-flash-builder/)

## Step 4: How do you know if your app is now universal/64-bit?

… before submitting it to the Apple App Store, that is. There are a couple of things you can check. First, if you build your app with AIR 16, you should **NOT** get an error message like this:

Error: Apple App Store allows only universal applications. “libMultiplatformANETemplateLib.a” is not a universal binary. Please change build settings in Xcode project to “Standard Architecture” to create universal library/framework.

Or like this:

\[exec\] Error: libCameraLibiOS.a are required to have universal iOS libraries. Please contact the ANE developer(s) to get the same.\[exec\] Result: 12

Then,  to double-check, do the following: **4.1.** Rename your **.ipa** file to **.zip** and unzip it. **4.2.** Use **lipo** in the **Mac Terminal**:

1

lipo path\_to\_your\_unzipped\_ipa/Payload/your\_app\_name.app/your\_app\_name \-info

You want to see a message like this:

1

Architectures in the fat file: your\_app\_name are: armv7 arm64

 **4.3.** You can also use the **file** command on **Mac**:

1

file path\_to\_your\_unzipped\_ipa/Payload/your\_app\_name.app/your\_app\_name

This should result in a message like this one:

1

2

3

Mach\-O universal binary with 2 architectures

your\_app\_name (for architecture armv7): Mach\-O executable arm

your\_app\_name (for architecture arm64): Mach\-O 64\-bit executable

Ta-da! Your app is now up to date with the 64-bit requirements.