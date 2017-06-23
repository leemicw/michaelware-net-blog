---
title: Comparing Efficiency of Two SQL Commands
tags: [SQL, Performance]
date: 2012-12-28 21:09:00
---

I am often working on SQL Server as my database backend.&nbsp; When trying to determine how efficient two different database queries might be I find it helpful to use statistics available to determine hard performance numbers instead of gut feel or using my trusty stop watch.&nbsp; When you run a query you get the two tabs shown below.&nbsp;&nbsp;

&nbsp;

![Blank Info](/content/images/2012/Comparing-Efficiency-of-Two-SQL-Commands/blankInfo.png)

&nbsp;

The messages tab usually does not have much information but you can change that with two simple commands.&nbsp;&nbsp; If you run the following two commands in your query window the messages tab will contain a lot more information about the resources your query required to complete.&nbsp;

SET STATISTICS IO ON 
SET STATISTICS TIME ON

After running the same simple “select * from tablename” query as above you get the following results.

![Stats Info](/content/images/2012/Comparing-Efficiency-of-Two-SQL-Commands/statsInfo.png)

There are three parts of this I often use.

### 1\. Scan Count

This gives you the number of table or index scans that were required to complete the query.&nbsp; In this case a single table scan is expected since this is pulling all data from a single table and there are no indexes on the table.&nbsp; If this number is large it can be an issue in performance.&nbsp; If this is the case I normally switch to looking at the execution plan, but that is for another blog post.&nbsp;

### 2\. Logical Reads and Physical Reads

These numbers tell us either the number of reads made to the storage device (physical reads) or the number of reads that would have been made to the storage device if the data had not been cached (logical reads).&nbsp; In this example the table was already in RAM on the SQL server so all the reads were logical.&nbsp;

### 3\. Elapsed Time

This is my favorite number since it gives me the ability to compare sub-second queries to each other.&nbsp; The example took 442ms (.4 seconds).&nbsp; If you had a query that was doing the same thing but took 800ms it would be pretty easy to choose from them.&nbsp;

## How to use these numbers

There are no absolute right and wrong numbers to see.&nbsp; Generally lower is better but there are times when scan count might be high but performance is not affected.&nbsp; You might see a query where the CPU time is greater than the elapsed time.&nbsp; This generally means the query is CPU intensive.&nbsp; This is also not necessarily a bad thing, just something you want to be aware of.&nbsp; Hopefully these commands will give you a little more information about your queries so you can make good decisions.&nbsp;