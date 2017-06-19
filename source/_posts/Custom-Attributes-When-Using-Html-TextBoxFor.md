---
title: Custom Attributes When Using Html.TextBoxFor
tags:
  - Software
date: 2010-01-31 21:52:00
---

I finnally got a few minutes to work on the new asp.net mvc 2 this weekend.&nbsp; I was working through the example on [Model Validation Scott Guthrie](http://weblogs.asp.net/scottgu/archive/2010/01/15/asp-net-mvc-2-model-validation.aspx) did on Jan 15th and found my self wanting to resize the text boxes on my input form.&nbsp; In his example he is using Html.TextBoxFor and calling into his model object for the attributes of the input tag.&nbsp; This seemed really nice and I worked with it.&nbsp; Since I wanted to resize the input boxes I started trying to put a class into the parameters of TextBoxFor since it has two overloads with htmlAttributes.&nbsp; After some fiddling I decided to google, this turned up some questionable advice such as "Don't be afraid of using raw HTML" which sounds a bit like hard coding it just because you don't know how.&nbsp; My search continued.&nbsp; I decide I would take a look futher down the comments of ScottGu's post and found the answer there (mostly).&nbsp; He said about half way down you should use the following syntax:

![](http://www.michaelware.net/image.axd?picture=2010%2f2%2fhtmlTextBoxForScottGuExample.jpg)

This seemed to work but I was trying to add a class attribute so I could affect the text boxes.&nbsp; I tried the following.&nbsp;

![](http://www.michaelware.net/image.axd?picture=2010%2f1%2fhtmlTextBoxForClassExampleBroken.jpg)

This will not compile.&nbsp; After a looking at this for a minute its clear that class is a keyword.&nbsp; Simple work around below.

![](http://www.michaelware.net/image.axd?picture=2010%2f1%2fhtmlTextBoxForClassExampleWorking.jpg)

This got me right where I wanted to be.&nbsp; I had a class attribute on my input boxes that I could reference in my css.&nbsp; This would be the end of a happy story if after applying my css it had updated the look of my text boxes.&nbsp; After a quick look in fire bug I found my page css was being overridden by a site level setting.&nbsp;

![](http://www.michaelware.net/image.axd?picture=2010%2f1%2ffirebugexample.jpg)

Since this was a change I would not mind having site wide I thought about chaning it there or adding a input[type="text"] to my pages css.&nbsp; The only draw back to this is IE6 does not support this style of input selector.&nbsp; I decided to go with the class element since it works in all the browsers I have to support (IE6 is still 20% of the traffic on our sites).

&nbsp;