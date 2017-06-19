---
title: Programmatically Updating App.config in Console Applications
tags:
  - Software
date: 2011-08-17 01:25:00
---

Do you remember [ini](http://en.wikipedia.org/wiki/INI_file) files?&nbsp; They seemed so easy to use in the old days.&nbsp; I think what was most appealing about them was the ease with which you could read and write to them either programmatically or in a text editor.&nbsp; I still use this simple configuration pattern for console applications where a full database is too much to setup and I don&rsquo;t want to use the registry because it stores settings in a different place than the exe file.&nbsp; Below are some notes on how I use the .net ConfigurationManager class to accomplish this.

## Reading Configurations

Reading configurations works the same way for console, WinForm, and asp.net apps.&nbsp; They all automatically load either the app.config or web.config associated with the application.&nbsp; Below is a sample appSettings section of a config file.

<span style="font-family: consolas;">&lt;appSettings&gt; 
&nbsp;&nbsp;&nbsp; &lt;add key="lastRunTime" value="8/16/2011 10:05:20 PM"/&gt; 
&lt;/appSettings&gt;</span>

To access the values you make use of the ConfigurationManager class.&nbsp; To get access to the ConfigurationManager please make sure you **add a reference to your project for System.Configuration** and the following using statement.

<span style="font-family: consolas;">using System.Configuration;</span>

Once those two items are in place you can start accessing items in your config file with the following syntax.

<span style="font-family: Consolas;">namespace AppConfigExample 
{ 
&nbsp;&nbsp;&nbsp; class Program 
&nbsp;&nbsp;&nbsp; { 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; static void Main(string[] args) { 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; DateTime lastRunTime = DateTime.Parse(ConfigurationManager.AppSettings["lastRunTime"]); 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; } 
&nbsp;&nbsp;&nbsp; } 
}</span>

This returns the string value in your config file so you may need to cast it depending on the datatype you want to work with.&nbsp; For example our lastRunTime is a DateTime so I could cast the value as follows.

<span style="font-family: consolas;">DateTime lastRunTime = DateTime.Parse(ConfigurationManager.AppSettings["lastRunTime"]);&nbsp; //please add error handing to production code</span>

This gives you a simple tool to store basic configurations for your program.&nbsp; On to the next step&hellip;

## Updating and Saving Configurations

In the previous example we had a setting for last run time.&nbsp; If this setting is going to be correct we will need to update the config file every time the application runs.&nbsp; Below is the code that will load the application configuration into a variable you can work with, update the lastRunTime setting to the current date, and save the file.

<span style="font-family: Consolas;">namespace AppConfigExample 
{ 
&nbsp;&nbsp;&nbsp; class Program 
&nbsp;&nbsp;&nbsp; { 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; static void Main(string[] args) { 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Configuration appConfig = ConfigurationManager.OpenExeConfiguration(Environment.GetCommandLineArgs()[0]); 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; appConfig.AppSettings.Settings["lastRunTime"].Value = DateTime.Now.ToString(); 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; appConfig.Save(ConfigurationSaveMode.Modified); 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; } 
&nbsp;&nbsp;&nbsp; } 
}</span>

OpenExeConfiguration needs the exe name for the application so it can pull the correct config file.&nbsp; A generic way to get the exe name is to use Environment.GetCommandLineArgs()[0].&nbsp; That way if you rename your application your code will continue to work.

## Summary

These two options should give you what you need to read and write simple configurations for your console applications.&nbsp; The config files for .net do have more information than a traditional ini file, but if you stick to working with the appSettings section you should not run into any issues.