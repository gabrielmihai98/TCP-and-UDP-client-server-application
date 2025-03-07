# TCP-and-UDP-client-server-application-for-message-management



I started the implementation from the framework provided by the university. 
Then I added the possibility for the server to read (to take input) from the 
keyboard, useful for the exit command. At the same time, I opened one more 
socket (in addition to TCP connections) for UDP connections.

After the TCP client is connected to the server, it immediately sends a message 
with its ID (clientID) so the server can keep track of all TCP clients. Server
saves all TCP clients along with the topics to which they are subscribed (and 
the SF option for each topic; SF = store & forward, so a client, once he is back
online, he wants to receive all the messages sent for him while he was offline), 
the status (online / offline), but also the number of topics to which they are 
subscribed. When a client connects, it checks in the "database" if it has been 
connected to the server. If it has not been connected, a new entry in the 
"database" is created for him. If it has already been connected, check the topics 
saved (in the server) with the topics to which the client has subscribed in the 
past. If there are such topics (common between those saved and those to which 
the customer is subscribed), after the moment of connection, all the messages 
that were "lost" while he was offline are sent to him.

When a UDP client sends a message, the bit string is "parsed" and is currently saved in a specific structure for each data type (of the 4).

- structure of type intDatagram for type INT;
- structure of type shortDatagram for type SHORT_REAL;
- structure of type floatDatagram for type FLOAT;
- structure of type stringDatagram for type STRING;

Then the content is formatted according to the other parameters and saved in a new
data structure ("store" type structure) together with the UDP client ip, but also 
the port. It is verified among the online clients at that time and the message is 
sent to all the online clients subscribed to the topic. In case there are offline 
clients at the moment, but subscribed to this topic with the option SF = 1, 
calculate the number of this type of clients and save the message (from the UDP 
client) in a special structure (type structure " storage "), together with the 
number of offline clients to which the message will have to be transmitted when 
they will be online again; this number will be decreased when a TCP client connects
and the corresponding message is sent to it.

By the time a message saved on the server reaches the number of offline 
clients = 0, this message is deleted from the server => the message has already 
been sent to all clients that were initially offline when the message was sent by 
the UDP client.

TCP clients have the option to subscribe or unsubscribe to a particular topic. If 
they type the wrong command for one of the 2 options, the server reacts by 
sending them a message specifying the correct format of the command.
