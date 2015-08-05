# Introduction #
I noticed that i've not explained how the system works. So here follows some more details.

# Details #
The project is based on a simple concept: the **AstCTIServer** connects to Asterisk Manager Interface with **one single connection** and intercepts Asterisk Events that are relevant for it's clients. These events include:

  * **Answer** - A call is answered
  * **Exten** - A new extension of the dialplan is executed
  * **Newchannel** - A new channel is built from asterisk
  * **Newstate** - A channel is changing it's status
  * **Newcallerid** - A callerid event
  * **Link** - Two channels are bound
  * **Hangup** - A call has been closed

Each of this events brings useful information. In particular, is important to notice that we can send informations from the dialplan to the **AstCTIClient** using the dialplan [Set](http://www.voip-info.org/wiki/index.php?page=Asterisk+cmd+Set) function: using the special calldata variable. This variable is grabbed by the **AstCTIServer** and sent to the AstCTIClient when the Link event occurs.

**AstCTIClient** is configured to receive events for a specified **technology/extension**: ex. SIP/201. The **AstCTIClient** will register - using TCP/IP sockets - on **AstCTIServer** and will receive relevant Asterisk events that happens on an active channel for the technology/extension for which it's registered.


Newstate events are used from AstCTIServer to notify AstCTIClient of channel status changes.

Newcallerid events are used to notify the CallerId of inbound calls.

Link events will indicates that the call is answered.
