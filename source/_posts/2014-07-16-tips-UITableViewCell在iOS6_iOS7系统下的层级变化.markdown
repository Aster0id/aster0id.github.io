---

layout: post
title: "iOS Tips: UITableViewCell在iOS6 iOS7系统下的层级变化"
date: 2014-07-16 14:53:38 +0800
comments: true
categories: iOS开发 Tips
tags: iOS Tips, UITableViewCell, iOS7
keywords: UITableViewCell, iOS7, iOS开发

---


在iOS6上UITableViewCell的层级为：
UITableViewCell —> UITableViewCellContentView

在iOS7上UITableViewCell的层级为：
UITableViewCell —> UITableViewCellScrollView —> UITableCellContentView


所以通过cell上的视图要想取到cell 必须做一个判定

```objective-c
	UITableViewCell *cell = IOS_VERSION<7.0 ? __SENDER__.superview.superview : __SENDER__.superview.superview.superview;
```