---
title: Using pool.ntp.org as Your External Time Source
tags:
  - Infrastructure
date: 2010-09-07 20:21:00
---

We recently ran into a situation that required our time be very close to everyone else in the world so we began to work on a solution to make sure all our equipment was using current ntp sources.&nbsp; We have two basic items to consider, windows servers and cisco asa appliances.&nbsp; Both are discussed below.

## Windows Servers (verifed on 2003 and 2008R2)

There are two variations (part of a domain and stand alone) in servers we looked at but they both have basically the same answer.&nbsp; Internal servers joined to a domain will get their time from one of the domain controllers so if the domain controllers are correct all the other servers will be as well.&nbsp;&nbsp;Non domain servers will all have to be&nbsp;configured to an authoritative source individually.

### Step 1 - Find&nbsp;The&nbsp;Domain Controller&nbsp;All the Other Domain Controllers are Referencing

According [How the Windows Time Service Works](http://technet.microsoft.com/en-us/library/cc773013(WS.10).aspx) the head of the time tree should be the PDC emulator.&nbsp; In our case it was and if you want to be sure you can&nbsp;run the following command which will produce&nbsp;something similar to the output below, depending on the number of domain controllers you have in your&nbsp;domain.&nbsp; This example contains four domain controllers.

<span style="font-family: courier new,courier;">w32tm /monitor
maindomaincontroller.company.com *** PDC ***[10.10.0.10:123]:
&nbsp;&nbsp;&nbsp; ICMP: 0ms delay
&nbsp;&nbsp;&nbsp; NTP: +0.0000000s offset from maindomaincontroller.company.com
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; RefID: maindomaincontroller.company.com [10.10.0.10]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Stratum: 1
secondarydomaincontroller1.company.com *** PDC ***[10.11.0.20:123]:
&nbsp;&nbsp;&nbsp; ICMP: 46ms delay
&nbsp;&nbsp;&nbsp; NTP: -0.0102363s offset from maindomaincontroller.company.com
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; RefID: maindomaincontroller.company.com [10.10.0.10]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Stratum: 2
secondarydomaincontroller2.company.com *** PDC ***[10.12.0.12:123]:
&nbsp;&nbsp;&nbsp; ICMP: 46ms delay
&nbsp;&nbsp;&nbsp; NTP: +0.0249849s offset from maindomaincontroller.company.com
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; RefID: maindomaincontroller.company.com [10.10.0.10]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Stratum: 2
secondarydomaincontroller3.company.com *** PDC ***[10.10.0.11:123]:
&nbsp;&nbsp;&nbsp; ICMP: 0ms delay
&nbsp;&nbsp;&nbsp; NTP: -0.0057875s offset from maindomaincontroller.company.com
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; RefID: maindomaincontroller.company.com [10.10.0.10]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Stratum: 2</span>

If you notice on line five this domain controller is referencing its self for time.&nbsp; This means you are not&nbsp;using an external time source and the listed domain controller is the one you need to modify.

### Step 2 - Finding an External Time Source

We found two options for this and actually ended up using both.&nbsp;

Option 1&nbsp;is the [ntp.org pool](http://www.pool.ntp.org/en/).&nbsp; This project allows people to donate server time to serve as ntp servers all over the world.&nbsp; This gives you access to server geographically located near you so you can cut down on network lag which can affect your abilty to keep your time correct.&nbsp;&nbsp;Since this project&nbsp;has many redudant nodes we decided it would be&nbsp;our first choice for an NTP source.&nbsp; &nbsp;See the ["use" page](http://www.pool.ntp.org/en/use.html) for determining the DNS names you should use.&nbsp;

Option 2 is the time servers provided by [NIST](http://www.nist.gov).&nbsp; These servers are considered reliable since they use the U.S. naval atomic clock to keep time but have a much smaller number of servers avalible as compared to the ntp.org pool.&nbsp; The names and IP addresses are [here](http://tf.nist.gov/tf-cgi/servers.cgi).

### Step 3 - Configure Your Server to Use the New Time Source

Changing domain controllers and stand alone servers requires the same command.&nbsp; We used the one below which works on Server 2003 and 2008.&nbsp; The first command we found on the ntp.org pool site only worked on Server 2003.&nbsp; Also, please make sure you select appropitate DNS records, the ones in this command are best suited for use in the United States.

<span style="font-family: courier new,courier;">w32tm /config /syncfromflags:manual&nbsp;/manualpeerlist:"0.us.pool.ntp.org 1.us.pool.ntp.org 2.us.pool.ntp.org 3.us.pool.ntp.org" /reliable:YES</span>

After running this command you should be able to run the command from step 1 and see the updated time server.&nbsp; You may also note your statums have changed denoting how many levels away from the root source you are.&nbsp;

<span style="font-family: courier new,courier;">w32tm /monitor
maindomaincontroller.company.com *** PDC ***[10.10.0.10:123]:
&nbsp;&nbsp;&nbsp; ICMP: 0ms delay
&nbsp;&nbsp;&nbsp; NTP: +0.0000000s offset from maindomaincontroller.company.com
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; RefID: externaltimesource.com [171.10.10.10]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Stratum: 3
secondarydomaincontroller1.company.com *** PDC ***[10.11.0.20:123]:
&nbsp;&nbsp;&nbsp; ICMP: 46ms delay
&nbsp;&nbsp;&nbsp; NTP: -0.0102363s offset from maindomaincontroller.company.com
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; RefID: maindomaincontroller.company.com [10.10.0.10]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Stratum: 4
secondarydomaincontroller2.company.com *** PDC ***[10.12.0.12:123]:
&nbsp;&nbsp;&nbsp; ICMP: 46ms delay
&nbsp;&nbsp;&nbsp; NTP: +0.0249849s offset from maindomaincontroller.company.com
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; RefID: maindomaincontroller.company.com [10.10.0.10]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Stratum: 4
secondarydomaincontroller3.company.com *** PDC ***[10.10.0.11:123]:
&nbsp;&nbsp;&nbsp; ICMP: 0ms delay
&nbsp;&nbsp;&nbsp; NTP: -0.0057875s offset from maindomaincontroller.company.com
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; RefID: maindomaincontroller.company.com [10.10.0.10]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Stratum: 4</span>

### Step 4&nbsp;- Verify&nbsp;Your New Time Source Is&nbsp;Working

If you like you can wait for your time to sync by itsself but if you restart the windows time service it should sync immediatly.&nbsp; If you do not get the correct time make sure your timezone is correct and your clock is not more than a few minutes off from the time the external server has.&nbsp; If its too far out of sync the windows time service will not change the time.

## Cisco ASA Appliances

After doing all the leg work on the windows server front we thought it would be easy to set the NTP setting on the ASA.&nbsp; The only snag we ran into was the firewall will not take a DNS name as its NTP server.&nbsp; Since we had to use an IP address we elected to go with servers from the NIST list as people participating in the ntp.org pool can choose to leave at anytime and the current IP address being served by the&nbsp;DNS name may not always be a time server. &nbsp;Screen shots of the config will have to wait for a later post.&nbsp;

## Other Reading:

*   [http://www.pool.ntp.org/](http://www.pool.ntp.org/)
*   Go to your Command Prompt and run this: "w32tm /?"
*   How the Windows Time Service Works [http://technet.microsoft.com/en-us/library/cc773013(WS.10).aspx](http://technet.microsoft.com/en-us/library/cc773013(WS.10).aspx)

&nbsp;