---
title: 'Comparing String Concatenation in C#'
tags:
  - Performance
  - 'C#'
date: 2019-08-16 21:50:30
---

I have always heard you should avoid string.format because it has to create a System.Text.StringBuilder object.  With the introduction of
string interpolation in C# I wanted to see how some of the different concatenation options performed.  

Here is the test code I ran in linqPad

<pre class="prettyprint">
var limit = 1000000;

var stopwatch = new Stopwatch();
stopwatch.Start();
for(var i = 0; i < limit; i++){
	var newString = $"This is a new{i} string with two {i} things inserted";
}
stopwatch.Stop();
"interpolation".Dump();
stopwatch.ElapsedMilliseconds.Dump();


stopwatch = new Stopwatch();
stopwatch.Start();
for(var i = 0; i < limit; i++){
	var newString = string.Format("This is a new {0} string with two {1} things inserted", i, i);
}
stopwatch.Stop();
"string.format".Dump();
stopwatch.ElapsedMilliseconds.Dump();


stopwatch = new Stopwatch();
stopwatch.Start();
for(var i = 0; i < limit; i++){
	var newString = "This is a new " + i + " string with two " + i + " things inserted";
}
stopwatch.Stop();
"concat".Dump();
stopwatch.ElapsedMilliseconds.Dump();


stopwatch = new Stopwatch();
stopwatch.Start();
for(var i = 0; i < limit; i++){
	var newString = string.Join("This is a new ", i, " string with two ", i, " things inserted");
}
stopwatch.Stop();
"string.Join".Dump();
stopwatch.ElapsedMilliseconds.Dump();
</pre>

Here are the results.  All the speed numbers are in milliseconds.

| Iterations   | Interpolation    | string.Format  |  concat  |  string.Join  |
| ---------- |-----------:| -----:|-----:|-----:|
|100| 0 | 0 | 0 | 0 |
|1,000| 0 | 0 | 1 | 0 |
|10,000| 4 | 5 | 3 | 4 |
|1,000,000| 401 | 405 | 310 | 368 |

For all the fuss, I really expected the string builder to be much slower.  My takeaway is you have to be above a million iterations before you can even see measureable differences.  Therefore, unless you have an actual production performance problem, write whatever is easiest to read.  The millisecond you save won't matter if it costs you even 30 extra seconds to understand what the code is doing. 

