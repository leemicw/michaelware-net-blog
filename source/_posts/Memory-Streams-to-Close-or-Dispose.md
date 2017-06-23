---
title: Memory Streams to Close() or Dispose()
tags: [C#]
date: 2014-04-07 23:09:00
---

I was at the .net users group meeting the other night and we were discussing code analysis.&nbsp; Just for fun I ran it on the project I was working on that day and ran across this warning.&nbsp; 

![Dispose Warning](/content/images/2014/Memory-Streams-to-Close-or-Dispose/DisposeWarning.png)

Here is (basically) the code that was causing it. 

![Code Sample](/content/images/2014/Memory-Streams-to-Close-or-Dispose/code.png)

I asked, why is this an issue?&nbsp; A couple of comments.

*   Are you disposing of it in a using statement?
*   Are you doing this in a try block and disposing of it in both the try and the finally?  

Neither of those is happening.&nbsp; 

The generally consensus was it should not do that because close never calls dispose, but after some research I see why it is.&nbsp; Since code analysis works on the IL code it can see things we can’t see on the surface.&nbsp; According to this [answer on stack overflow](http://stackoverflow.com/a/7525134/203963) the close method is calling dispose for you.&nbsp; That’s handy right?&nbsp; How about [this from the MSDN documentation](http://msdn.microsoft.com/en-us/library/system.io.stream.close.aspx), emphasis mine.
  > Closes the current stream and releases any resources (such as sockets and file handles) associated with the current stream. **Instead of calling this method, ensure that the stream is properly disposed**.  

That seems reasonable, apart from having a public method I am told in the docs not to call.&nbsp; Going too far the other way removing both the .Close() and .Dispose() leaves us with this message.&nbsp; 

![No Dispose Warning](/content/images/2014/Memory-Streams-to-Close-or-Dispose/NoDisposeWarning.png)

<font color="#0789c9"></font>

I am going to go with just .Dispose().&nbsp; Today its not really different from calling both, but in the future it might be and the docs say to do it this way.&nbsp; Plus, it gets rid of the nagging warning.&nbsp; 