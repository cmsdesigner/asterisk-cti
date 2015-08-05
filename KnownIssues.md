# Introduction #
There are some known issues with mono runtime.
  1. System.Net.Dns.GetHostEntry ignoring provided IP address
  1. MySQL 5.0.x requires MySql.Data.dll version 5.0.9


# Details #
## System.Net.Dns.GetHostEntry ignoring provided IP address ##
_GetHostEntry is invoking the native C mono code to resolve the host and IP addresses of the provided HostNameOrAddress. When this parameter is already an IP this value should be used by default. This bug shows himself when the machine has complex networking components like bridged interfaces. In my scenarion I had an bridge to make a VirtualBox virtual machine accessible from the rest of my network and in the end mono GetHostEntry returned the 127.0.0.1 altough I provided 192.168.2.94 as the host_
name.

**Please check this [Bug Report](https://bugzilla.novell.com/show_bug.cgi?id=351908)**

## MySQL 5.0.x requires MySql.Data.dll version 5.0.x ##
If you're using MySql Server 5.0.x, please download Connector/Net 5.0 from [here](http://dev.mysql.com/downloads/connector/net/5.0.html).


