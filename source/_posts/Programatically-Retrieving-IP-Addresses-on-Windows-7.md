---
title: Programatically Retrieving IP Addresses on Windows 7
tags:
  - Software
date: 2010-02-17 21:41:00
---

We ran into an interesting problem while testing our apps for windows 7.&nbsp; Our WinForms apps kept throwing a "String or binary data would be truncated. The statement has been terminated."&nbsp; This is a pretty common error for SQL server to throw when you push a string that is longer than the column into the insert or update.&nbsp; We were only seeing this on Windows 7 and it was coming from code we had been successfully running on XP for quite some time.&nbsp; It turns out to be some code that we used to pull the IP Address of the machine running the application.&nbsp; The old code is below.

![](http://www.michaelware.net/image.axd?picture=2010%2f2%2fOld.png)

The problem with this is its pulling the first IP address found on the machine.&nbsp; Windows 7 has IPv6 enabled by default.&nbsp; This code started pulling the much longer IPv6 address which resulted in overflowing the column in SQL.&nbsp; To correct this we updated the code to the following.

![](http://www.michaelware.net/image.axd?picture=2010%2f2%2fNew.png)

By comparing to the AddressFamily enum we are able to look at only version 4 addresses.&nbsp; It's interesting to note some of the other options in the enum options including some protocols I did not think I would see again.