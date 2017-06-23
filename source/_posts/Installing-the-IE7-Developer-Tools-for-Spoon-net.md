---
title: Installing the IE7 Developer Tools for Spoon.net
tags: [Testing]
date: 2013-04-17 22:37:00
---

I have been using spoon.net for quite a while for my cross browser testing needs.&nbsp; One thing that seemed to be lacking was the Developer Tools in IE7.&nbsp; IE8 and IE9 both had the developer tools out of the box so they were easy to find using spoon.&nbsp; I assumed getting the tools added to IE7 would not be possible since it was originally a separate download from Microsoft.&nbsp; Not so!&nbsp; Since (as I understand it) spoon installs a comparability shim onto your computer for things that are no longer supported but uses most of the resources of your local machine, you can install the IE7 developer tools and the spoon browser will allow you to use them.&nbsp;

### Install Procedure

Download the ie7 developer tools from [here](http://www.microsoft.com/en-us/download/details.aspx?id=18359).&nbsp; It says it requires Windows XP but installs fine on my Windows 8 Box.

On the downloaded file right click, choose properties, and unblock on the downloaded file

Install the application (this requires admin rights)

Launch IE7 from Spoon and you can activate the tools by going to the following menu: View-&gt;Explorer Bar-&gt; IE Developer Toolbar

![Toolbar Location](/content/images/2013/Installing-the-IE7-Developer-Tools-for-Spoon-net/ToolbarLocation.png)

Happy Testing!