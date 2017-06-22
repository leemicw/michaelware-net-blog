---
title: Tips for Setting up SSAS Access over IIS/HTTP
tags: [IIS, SSAS, Windows Server]
date: 2015-05-28 20:09:00
---

One of what I think is a limiting factor for Analysis Services is its lack of a “standard” authentication mechanism.&nbsp; There are lots of scenarios where windows authentication just isn't practical.&nbsp; Fortunately we can setup access to an SSAS server over HTTP with IIS and and ISAPI filter.&nbsp; This MSDN article does a great job explaining the steps you need to take.&nbsp; 

[https://msdn.microsoft.com/en-us/library/gg492140.aspx](https://msdn.microsoft.com/en-us/library/gg492140.aspx "https://msdn.microsoft.com/en-us/library/gg492140.aspx")

We setup an IIS server running on Windows Server 2012 R2 connecting to SSAS 2014.&nbsp; The only issue I found is during the connection testing phase.&nbsp; Our excel test worked perfectly from outside our network but we kept getting the following error when connecting from C# using ADOMD.&nbsp; 

_The integrated security 'Basic' is not supported for HTTP or HTTPS connections._

Now I just read in the article how basic authentication is supported so you can allow access from machines outside your network.&nbsp; The good news is this isn't referring to that basic authentication.&nbsp; Here is the connection string recommended by the MSDN article. 

_Data Source=https://&lt;servername&gt;/olap/msmdpump.dll; Initial Catalog=AdventureWorksDW2012; <font style="background-color: #ffff00">Integrated Security=Basic;</font> User ID=XXXX; Password=XXXXX;_

The highlighted part is what’s causing the error.&nbsp; Remove "Integrated Security=Basic;" and your connections should work fine.&nbsp; We were able to connect over http, https, and by using a high number port above 5000 just by changing the IIS website bindings and the url in the connection string.&nbsp; Overall I have been very happy with how this is working out. 