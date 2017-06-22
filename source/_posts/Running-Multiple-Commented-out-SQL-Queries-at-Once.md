---
title: Running Multiple Commented out SQL Queries at Once
tags: [SQL]
date: 2015-04-15 19:52:00
---

So I do this a lot.&nbsp; I have a couple of select statements that are commented out in my SQL script where I can check the outcome of some update or insert operation.&nbsp; These statements are usually right above whatever script they are meant to verify.&nbsp; Today I had two queries I wanted to run to see if the outcome was correct and I wanted them run at the same time.&nbsp; I didn’t think this was possible, but it turns out it is.&nbsp; Since the select statements are both commented out with -- all you need is the first couple of characters of the queries removed and they can both run.&nbsp; Square select is your friend here. It can select both lines at once without having to remove either of the comment marks and SQL management studio is smart enough to run just the selected parts of the queries even when the selection is through square select.&nbsp; Here’s an example from the AdventureWorksDW database. 

[![SquareSelectSQLExample](http://www.michaelware.net/image.axd?picture=SquareSelectSQLExample_thumb.gif "SquareSelectSQLExample")](http://www.michaelware.net/image.axd?picture=SquareSelectSQLExample.gif)

If you haven't used square select, try it out.&nbsp; Its great in all sorts of cases and works in SQL Management studio, Visual Studio and most great text editors including Notepad++.&nbsp; Just hold down Alt+Windows Key and start making a selection.&nbsp; 