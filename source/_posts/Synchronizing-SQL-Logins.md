---
title: Synchronizing SQL Logins
tags: [SQL]
date: 2011-05-12 20:47:00
---

## Update 2017-06-20

This does not work on Sql Azure.  I believe the replacement is <pre>alter user</pre>

## Original Article 

Do you ever move a database from one server to another?&nbsp; If you do chances are your user ids will get out of sync since the access is granted to a sid instead of a username.&nbsp; If after restoring your database you cannot login you can run the following command to see a list of orphaned user ids in your database.

<span style="font-family: Consolas;">sp_change_users_login 'report'</span>

Once you see the name of the orphaned user you can choose a user on the new server to connect it to. Run the following script to link the old user to the new user.

<span style="font-family: Consolas;">sp_change_users_login 'update_one','OriginalUserFromFirstServer','DestinationUserOnNewServer'</span>

I normally like these user names to match so the second and third arguments are almost always the same.