---
title: Single Quotes/Ticks in Content-Disposition Header
tags: [asp.net, C#, IIS]
date: 2013-11-12 08:54:00
---

We got a bug report on an website today that people could no longer open files we store in a SQL database.&nbsp; After a little research we determined the issue was only in IE10 and IE11.&nbsp; Here is what the download message looked like.&nbsp;

![Download Message](/content/images/2013/Single-Quotes-Ticks-in-Content-Disposition-Header/downloadMessage.png)

Notice the extra single ticks around motorists.jpg?&nbsp; When you download a file and the extension is .jpg’ or .zip’ you get unpredictable results.&nbsp; Here is the line of code that was causing the issue.&nbsp;

![Content Broken](/content/images/2013/Single-Quotes-Ticks-in-Content-Disposition-Header/contentBroken.png)

Here is the fixed line of code.&nbsp; Just removing the single ticks from around the file.

![Content Fixed](/content/images/2013/Single-Quotes-Ticks-in-Content-Disposition-Header/contentFixed.png)