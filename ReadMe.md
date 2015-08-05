# 1) SETUP YOUR DATABASE #
First of all, you need a working MySQL. In my tests, I've used mysql 5.0.x. You may need to setup a database before load the Sql needed. For my tests, I use the predefined 'test' database. Within your database, You've to create a table called 'cti'.

Here is the Sql syntax (check also the file Docs\cti.sql):

```
CREATE TABLE `cti` (
  `USERNAME` varchar(255) default NULL,
  `SECRET` varchar(255) default NULL,
  `HOST` varchar(255) default NULL,
  `EXT` varchar(255) default NULL,
  UNIQUE KEY `USERNAME` (`USERNAME`),
  UNIQUE KEY `EXT` (`EXT`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

This table will contains credential used by the AstCTIServer to authenticate the clients. The passwords are stored as MD5 hash. After creating the cti table, you need to create almost one user. Sorry, but there's no configuration panel yet, so you've to do this job by hand using sql syntax like the following:

```
INSERT INTO cti VALUES('test',MD5('test'),'0.0.0.0','');
```

The AstCTIClient will update the database with the informations required by the AstCTIServer during logon.

Once the database and the cti table are ready, you'll need to write down mysql user and passsword to setup the server and the clients. I suggest you to create two different mysql acconts, one for the server and the other one for the clients. This document doesn't cover mysql user creation.

# 2) SETUP AstCTIServer #
The AstCTIServer requires a .NET 2.0 enabled platform to run. I've tested the server on the following configurations:

  1. Microsoft Windows XP SP 2, Microsoft .NET Framework 2.0
  1. Linux Fedora Core 7, Kernel 2.6.22, Mono 1.2.5
  1. Gentoo Linux, Kernel 2.6.22, Mono 1.2.4

Before to start the AstCTIServer, you need to configure some parameters from the XML configuration file.
The configuration file is splitted in four main sections: database, logging, manager, ctiserver.

  * database - contains MySQL database configuration
  * logging - contains the settings for file logging
  * manager - configuration for asterisk manager interface (AMI) connection. (check the manager.conf file in asterisk: you should have configured an account for AstCTIServer connection)
  * ctiserver - server socket configuration parameters.

Change the values to match your needs.

The AstCTIServer is not yet a Windows Service or a Unix Daemon. So the main application thread will not release the console where it runs. To start AstCTIServer, you can:
(on Windows) do a double-click on the AstCTIServer or open a console and issue the AstCTIServer.exe command from the path where the exe is. Or you can do a batch file to start the program
(on Linux) open a console, cd to the AstCTIServer.exe directory and give the following command: mono AstCTIServer.exe

If all is right configured, the server should start (check the logs in the logs/ directory) and you should see a manager connection on your asterisk console.

# 3) ASTERISK DIALPLAN #
In order to get the AstCTIClient work, we can do a basic dialplan configuration.
In the directory Docs/Demo you'll find the following files:

  * asterisk/manager.conf
  * asterisk/extensions.conf
  * asterisk/sip.conf
  * asterisk/queues.conf
  * asterisk/sounds/astctidemo/enter\_five\_digits.wav

First of all, you should check that asterisk AMI accepts connections from AsteriskCTI Server. So you’ve to add an user to the file asterisk/manager.conf that will match AstCTIServer configuration. Check demo file for an example.
You can copy dialplan demo from the file extension.conf and paste it into your asterisk dialplan. Then you've to copy sip agents from sip.conf to your asterisk sip.conf file and the same for the queues.conf. Adjust all the parameters to match your needs.

After this, copy the folder asterisk/sounds/astctidemo to your /var/lib/asterisk/sounds directory.

Restart your asterisk dialplan (`asterisk -rx "extensions reload"`).

The demo consists of two things:
**1) Demo at extension "100":**

The IVR asks for 5 digits. The 5 digits are stored in a variabile called "calldata" and then the call is sent to SIP/201.

**2) Demo at extension "101":**

The same as extension 100, but the call is sent to a queue (with only one agent, SIP/201).

In both demo, the important part is the function:

```
exten => 101,3,Set(calldata=${cdata})
```

This function is read from AstCTIServer and the value of the calldata variable is then sent to the AstCTIClient once the Link event occurs. So you've to always set a variable called "calldata" before send the call to the clients.

# 4) SETUP AstCTIClient #
AstCTIClient is a Win32 application written in C# for .NET 2.0. I think that the application can be easly ported to mono with GTK. I'm searching for volunteers...

The application User Interface is quite easy. Once started, the client needs to be configured. To do so, click the small Settings button on top of the window: should appear an "Application Settings" window.
The Application Settings are divided in Five Categories:

1) CTI APPLICATION
  1. **CalleridFadeSpeed** - The speed of fadein effect of the callerid form
  1. **CalleridTimeout** - How many seconds the callerid form should be shown before disappear
  1. **TriggerCallerid** - Show the callerid form?
  1. **CTIContextes** - Inbound calls contextes to match
  1. **CTIOutboundContextes** - Contextes for outbound calls (with originate)

2) CTI SERVER
  1. **Host** - hostname or ip of the AstCTIServer
  1. **Port** - port where the AstCTIServer is bound
  1. **Username, Password** - Credential as configured in the "cti" table of the database
  1. **PhoneExt** - Phone extension to match (ex. SIP/201)
  1. **SocketTimeout** - Timeout of client socket

3) DATABASE
  1. **MySQLHost** - the hostname or ip of MySQL Database
  1. **MySQLUsername** - Username for MySQL Database
  1. **MySQLPassword** - Password for MySQL Database
  1. **MySQLDatabase** - the database to access

4) INTERFACE
  1. **MinimizeOnStart** - When true, once the AstCTIClient have done a succesful login, it minimize automatically on tray area

5) MISC
  1. **SaveOnClose** - Save all settings done when the "Application Settings" window is closed.

CTIContextes are inbound calls contextes to match. For each context we can start a different application (with different parameters). For our example, add a context called "astctidemo" like the context of asterisk demo dialplan. Set it to enable, give it a name in the "DisplayName" field, and finally add your Application. In the example, you can choose "Internet Explorer" from the directory "c:\program files\Internet Explorer\iexplorer.exe". Add some Parameters like this: "http://centralino-voip.brunosalzano.com/demo_page.php?callerid={CALLERID}&calldata={CALLDATA}&channel={CHANNEL}&uniqueid={UNIQUEID}"

When a calls arrive in the astctidemo context for the extension configured on "CTI SERVER/PhoneExt", Internet explorer will be started when the SIP/201 answers the call.

CTIOutboundContextes are contextes where we can originate outbound calls. In the example, you can configure an "astctidemo" outbound context to make calls to all extension between 200 and 299 directly from AstCTIClient.

Once all the parameters are well setup, make sure that AstCTIServer is running and that it's connected to asterisk. Then make a login with the right credential.

Now you can make some test calls from another phone to the extension 100 or 101... Good Luck!