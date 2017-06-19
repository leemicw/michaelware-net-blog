---
title: Single Quotes/Ticks in Content-Disposition Header
tags:
  - Software
date: 2013-11-12 08:54:00
---

We got a bug report on an website today that people could no longer open files we store in a SQL database.&nbsp; After a little research we determined the issue was only in IE10 and IE11.&nbsp; Here is what the download message looked like.&nbsp;

[![image](http://www.michaelware.net/image.axd?picture=image_thumb_3.png "image")](http://www.michaelware.net/image.axd?picture=image_3.png)

Notice the extra single ticks around motorists.jpg?&nbsp; When you download a file and the extension is .jpg’ or .zip’ you get unpredictable results.&nbsp; Here is the line of code that was causing the issue.&nbsp;

[![image](http://www.michaelware.net/image.axd?picture=image_thumb_4.png "image")](http://www.michaelware.net/image.axd?picture=image_4.png)

Here is the fixed line of code.&nbsp; Just removing the single ticks from around the file.

[![image](http://www.michaelware.net/image.axd?picture=image_thumb_5.png "image")](http://www.michaelware.net/image.axd?picture=image_5.png)