## Wednesday, 1/3 Socket to Me by Sonal Parab

**Tech News:** [NAME] (link)

### Network Ports  
Allow a single computer to run multiple services.  
* A socket combines an IP address and port.  
  
Each computer has 2^16 (65,536) ports.   
Some ports are reserved for specific services.
* 80: http  
* 22: ssh  
* 443: ssl

You can select any port, as long as it wont conflict with a service running on the desired computer.   
* ports < 1024 are reserved and should generally not be used
* `/etc/services` will have a list of registered ports for your local system

### Network Connection Types

#### Stream Sockets  
* Reliable 2 way communication.  
* Must be connected on both ends.  
* Data is received in the order it is sent. (not as easily done as it sounds)  
* Most use the Transmission Control Protocol (TCP).  

#### Datagram Sockets  
* "Connectionless" - an established connection is not required.  
* Data sent may be received out of order (or not at all).  
* Uses the User Datagram Protocol.    

---
## Tuesday, 1.2.18: Socket to Me, by Ida Wang

**Tech News:** [Gaming addiction classified as disorder by WHO](http://www.bbc.com/news/technology-42541404)

### Socket
A connection between 2 programs over a *network*.  
A socket corresponds to an IP (internet protocol) Address / Port pair.

#### To use a socket
1. create the socket (kind of like creating the WKP)
2. bind it to an address and port
3. listen (creator) / initiate a connection (client)
4. send / receive data

### IP Addresses
*All* devices connected to the internet have an IP address.  
IP addresses come in 2 flavors, IPv4 (the main standard) and IPv6 (what should be the main standard).  
Addresses are allocated in blocks to make routing easier.

**IPv4:** 2 byte addresses of the form `[0-255].[0-255].[0-255].[0-255]`  
- Each group is called an *octet*.  
- At most there are 2^32, or ~4.3 billion IPv4 addresses.

**IPv6:** 16 byte addresses of the form: `[0-ffff]:[0-ffff]:[0-ffff]:[0-ffff]:[0-ffff]:[0-ffff]:[0-ffff]:[0-ffff]`  
- Each group is known as a *hextet* (although not as standard as the octet).  
- Leading 0s are ignored.  

Any number of consecutive all 0 hextets can be replaced with `::`

For example:  
`0000 : 0000 : 0000 : 0000 : 004f : 13c2 : 0009 : a2d2`  
can also be written as  
`:: 4f : 13c2 : 9 : a2d2`

---
## Monday, 12/18 Always tip your servers by Jerome Freudenberg

**Tech news:** [Twitter cracks down on hate](https://www.washingtonpost.com/news/the-switch/wp/2017/12/18/twitter-purge-suspends-account-of-far-right-leader-who-was-retweeted-by-trump/?utm_term=.56ee6be9cd9b)

#### Homework Tips
* You can pass variable (ACK) to confirm connections between server and client
  * a more robust method would be to pass an integer and perform an operation on it
* Put the server in a forever loop and then place a while loop inside that runs until the client exits
  * Since the pipe is not removed, use sighandler to remove it then exits

#### Forking Server/Client Design Pattern
* Handshake
  1) Client connects to server and sends the private FIFO name. Client waits for a response from the server.
  2) Server receives client’s message and forks off a subserver
  3) Subserver connects to client FIFO, sending an initial acknowledgement message
  4) Client receives subserver’s message, removes its private FIFO

* Operation
  1) Server removes WKP and closes any connections to the client
  2) Server recreates WKP and waits for a new connection
  3) Subserver (wkp) and client (pp) send information back and forth

* Pros and Cons
  * Simple way to handle multiple clients at once
  * Downside is a lack of communication among clients and subservers (which may not be necessary)


---