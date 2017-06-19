---
title: 'Subtracting Dates in C# and SQL'
tags:
  - Software
date: 2010-03-04 20:40:00
---

I know this is really basic stuff but I have seen too many examples of someone trying to implement their own date class to do something simple like subtract one day from today's date.&nbsp; I also remember the day (long ago) someone showed me using negative numbers worked in the dateadd function of sql server.&nbsp; So in case you have not found these functions yet, here are some samples.

**<span style="font-size: medium;">C#</span>**

![](http://www.michaelware.net/image.axd?picture=2010%2f3%2fCSharpSample.png)

This gives you a DateTime object with yesterday's date.&nbsp; (Note it will be yesterday at the current time)

**<span style="font-size: medium;">SQL</span>**

![](http://www.michaelware.net/image.axd?picture=2010%2f3%2fSQLExample.png)

Gives this result:

![](http://www.michaelware.net/image.axd?picture=2010%2f3%2fSQLExampleResults.png)

I think these are good examples of why you cannot forget the basics of math when you are working on a giant calculator.&nbsp;

Hope this helps.