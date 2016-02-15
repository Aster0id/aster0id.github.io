---

layout: post
title: "解决Xcode5.1下编译报错（第三方库不支持arm64问题）  "
date: 2014-04-03 19:30:01 +0800
comments: true
categories: iOS开发 tips
tags: iOS开发 tips Xcode 编译 ARM
keywords: ARM64, iOS开发, Xcode, linker command failed with exit code 1 (use -v to see invocation)

---



今天升级 Xcode 到 5.1版本，在开发编译时过程中遇到一个问题如下：

```
"_UMShareToTencent", referenced from:
     -[NMPostsDetailViewController _actionShare:] in NMPostsDetailViewController.o
 "_UMShareToWechatSession", referenced from:
     -[NMPostsDetailViewController _actionShare:] in NMPostsDetailViewController.o
 "_UMShareToWechatTimeline", referenced from:
     -[NMPostsDetailViewController _actionShare:] in NMPostsDetailViewController.o
 ld: symbol(s) not found for architecture x86_64
 clang: error: linker command failed with exit code 1 (use -v to see invocation)

```

Xcode 5.0 默认情况下可以选择不进行arm64编译,但在Xcode5.1中做出了更改。

查阅文档，发现 Xcode 5.1 [发布说明](https://developer.apple.com/library/mac/releasenotes/DeveloperTools/RN-Xcode/Introduction/Introduction.html)中有这样一段描述：


```
Changes
1.Building
Arm64 is now included in the “Standard architectures” setting.
Xcode 5.0 introduced support for building 64-bit iOS applications but it was not enabled by default. To enable the option of building 64-bit in Xcode 5.0, an architectures setting was provided: “Standard Architectures Including 64-Bit” (ARCHS_STANDARD_INCLUDING_64_BIT).
 
With the introduction of Xcode 5.1, arm64 is included in the default "Standard architecture” build setting. This results in projects using the default setting automatically building for arm64 along with the standard 32-bit architectures.

Note: Be aware of the following architectures issues when opening your existing projects in Xcode 5.1:
When building for all architectures, remove any explicit architectures setting and use the default Standard Architectures setting. For projects that were previously opted-in using “Standard Architectures           Including 64-Bit”, switch back to the “Standard architectures” setting.
When opening an existing project for the first time, Xcode 5.1 may display a warning about the use of the Xcode 5.0 architectures setting. Selecting the warning provides a workflow to revise the setting.
Projects not able to support 64-bit need to specifically set the architectures build setting to not include 64-bit.
 
2.Projects configured to use ”Standard Architectures Including 64-bit” will be converted to “Standard Architectures $(ARCHS_STANDARD).


```

###解决方案：
选择 Targets -> Build Settings -> Architectures.  选择other

{% img /images/2014-04-03-solve-error-of-xcode5.1-compiling/build-setting-arc.jpg %}  


删除$(ARCH_STANDARD),然后添加armv7和armv7s

{% img /images/2014-04-03-solve-error-of-xcode5.1-compiling/resetting-arc.jpg %}  

然后Clean,再编译就好了
