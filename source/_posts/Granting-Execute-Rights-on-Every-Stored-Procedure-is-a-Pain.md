---
title: Granting Execute Rights on Every Stored Procedure is a Pain
tags: [SQL]
date: 2014-05-30 20:52:00
---

I did this again the other day when setting up a database so I thought I would share. &nbsp;In SQL server its pretty easy to grant someone read access or write access to all the tables. &nbsp;You just have to assign them the data_reader or data_writer database role. &nbsp;Since most of the projects I work use a custom ORM that back ends into stored procedures I wanted the same simplicity of granting execute rights for all stored procs in the database. &nbsp;I give you db_executor.&nbsp;

<pre class="prettyprint">/* CREATE A NEW ROLE */
CREATE ROLE db_executor

/* GRANT EXECUTE TO THE ROLE */
GRANT EXECUTE TO db_executor
</pre>

<div>Once you have run this in your database you can assign the role to any user who needs to execute all stored procs. &nbsp;The nice part about this role compared to selecting all the stored procs and granting individual execute privilege to them as you add new stored procs the users in this role will automatically have rights to execute them.&nbsp;</div>