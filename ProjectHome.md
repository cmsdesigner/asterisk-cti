# Asterisk CTI #
## Introduction ##
CTI for Asterisk I have seen a few and they are all commercial. For this reason I have decided to initiate a project to develop a client-server solution that meets a series of requirements CTI base.

See Featured downloads for stable Release.

---

**For Latest Release**:
[AstCTIClient](http://asterisk-cti.googlecode.com/svn/trunk/Setups/AstCTIClient/AstCTIClientSetup.exe)
[AstCTIServer](http://asterisk-cti.googlecode.com/svn/trunk/Setups/AstCTIServer/AstCTIServerSetup.exe)

---

# Updated version #
There's an update to installers and setups: please upgrade your previous version.
**PLEASE NOTE**: the installers are always older than SVN versions.
**To check latest updates and improvements**, take a look at [Wiki SVN Updates Page](http://code.google.com/p/asterisk-cti/updates/list).

## Feedback ##
**Plese send your feedbacks** in the [Wiki Feedback Page](http://code.google.com/p/asterisk-cti/wiki/FeedBack)

The software is still in Alpha. Use it at your own risk!

**YOU NEED BOTH SERVER AND CLIENT TO GET THE SYSTEM WORKS**

## Project Architecture ##
### CTI Server ###

  * Platform development:. Net 2.0 / Mono on GNU/Linux
  * Integration with the Asterisk Manager Interface (single connection)
  * Multithread engine for the management of numerous CTI clients
  * Configuration Files to manage operating parameters
  * Mudulable Log File
  * Updated MySql.Data

### CTI Client ###

  * Platform development:. Net 2.0 for Windows
  * Authentication on CTI Server
  * Attractive and user-friendly GUI
  * Ability to manage the display Caller ID / Caller ID Name
  * Ability to originate callson different Outbound contextes
  * Advanced Configuration Interface
  * Ability to manage different telephone campaigns through the takeover of the “context”
  * Ability to launch diversified applications for telephone campaign
  * Each application can be parameterized. Different variables are supported for expansion, including a special variable “calldata” to transfer information from dialplan of CTI asterisk to Client
  * Localization Support (Italian / English / Spanish / Mexican / French / Russian). More translations welcome!
  * Integrated configurable Web browser
  * Registry settings to block configuration for users and/or outbound contextes

## System Requirements ##
### AstCTIServer ###
  * A working Asterisk 1.4.x installation :)
  * I suggest Mono on server side, but you can use also a separate Windows Server with a .NET 2.0 installation for AstCTIServer;
  * [MySQL Connector/Net](http://dev.mysql.com/downloads/connector/net/)
  * MySQL 5.x database Server

### AstCTIClient ###
  * Windows XP / Windows Vista
  * Microsoft .NET 2.0
  * [MySQL Connector/Net](http://dev.mysql.com/downloads/connector/net/)

