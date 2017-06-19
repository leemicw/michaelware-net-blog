---
title: 'Using C# Return Codes in Batch Files'
tags:
  - Software
date: 2011-02-10 20:19:00
---

Its been common in my experience to use batch or command files for automation of simple tasks.&nbsp; Sometimes you need just a little more flexibility than the batch language has.&nbsp; One way to deal with this is to write a command line program to handle the extra work.&nbsp; Since error levels can be used in batch files to control flow it would be nice to be able to communicate the success or failure to of the command line program back to the batch file.&nbsp; It turns out this is really easy.&nbsp; Below is the basic command line program you get from visual studio.

<span style="font-family: Consolas;">namespace ConsoleApplication1 
{ 
&nbsp;&nbsp;&nbsp; class Program 
&nbsp;&nbsp;&nbsp; { 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; static void Main(string[] args) { 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; } 
&nbsp;&nbsp;&nbsp; } 
}</span>

The main function has a return type of void.&nbsp; If we change that to int then we have the opportunity to give some information back to the batch file.&nbsp; The example below has the return value changed and does a simple comparison to see which number to return to the batch file.

<span style="font-family: Consolas;">namespace AppExitTest 
{ 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; class Program 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; { 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; static int Main(string[] args) { 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; int returnVal = 0; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if(args[0] == "true") { 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; returnVal = 23412341; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; } 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return returnVal; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; } 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; } 
}</span>

This returns an int which can be read out of the %ERRORLEVEL% variable in a batch file.&nbsp; Below is a batch file which demonstrates both paths in the command line program.

<span style="font-family: Consolas;">echo off 
appexittest.exe false 
echo %ERRORLEVEL% 
appexittest.exe true 
echo %ERRORLEVEL%</span>

This batch file gives the result:

<span style="font-family: Consolas;">echo off 
0 
23412341 
</span>