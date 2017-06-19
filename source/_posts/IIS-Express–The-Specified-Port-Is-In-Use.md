---
title: IIS Express–The Specified Port Is In Use
tags: []
date: 2015-12-21 08:40:30
---

So you’re in the zone, been coding all day, have all tools in just the right spot.&nbsp; Then BAM!&nbsp; The dreaded port is in use. 

[![Error1](http://www.michaelware.net/image.axd?picture=Error1_thumb.png "Error1")](http://www.michaelware.net/image.axd?picture=Error1.png)

Reading about this issue on stack overflow some people say its been chrome that has the port, or sometimes another process.&nbsp; For me, It’s always some orphaned or hung IIS Express process holding onto the port.&nbsp; Rarely, trying to start the debugger again will let you continue, but most of the time I had to resort to a reboot.&nbsp; This is obviously inconvenient and a waste of time.&nbsp; So here’s another option.&nbsp; 

## Get PS Kill

The [PS Tools](https://technet.microsoft.com/en-us/sysinternals) are awesome you should check them out.&nbsp; All we need today is PSKill.&nbsp; Install/Extract is as you see fit and make sure you can access it from the command line.&nbsp; 

## Netstat knows who is using the port

Run this from the command line.

netstat –ano | findstr &lt;port&gt; (in this case netstat –ano | findstr 62646)

The n is really important.&nbsp; It prevents trying to resolve all the DNS names of the ip addresses you have connections to.&nbsp; Without it, this command is so slow, it may appear broken.

Your should get one results like this.&nbsp;&nbsp; 

TCP&nbsp;&nbsp;&nbsp; 0.0.0.0:62646&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; D2000:0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; LISTENING&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2076

What your looking for is the number at the end of the line.&nbsp; It’s the PID of the process that currently has the port open. 

## Use PS Kill

Once you know the PID of the process you need to kill you can use pskill to stop it.&nbsp; 

Open an administrative command prompt, then run pskill &lt;pid&gt; (in this example pskill 2076)

Note: On very rare occasions you will see a PID or 0 or 1 (I forget off the top of my head.&nbsp; This is the NT system process.&nbsp; I do not recommend trying to kill it)