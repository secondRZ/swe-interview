# Networking

## Process

* Say you wanted to send a file from one computer to another. The file would be broken up into packets \(about 1 KB in size\). \(Remember that data is simply organized bytes. 0s and 1s. So a packet of organized binary code\) Each packet contains a from" field, a "to" field designating the address of the receiving computer, the bytes, and the checksum.
* Ethernet:
  * The sender waits for a silence on the wire \(maybe other computers on the network are communicating\) then broadcasts the message.
  * The packets are translated to the wire via voltage and sent through like a train. \(Say for a 1 the point in the wire has 3volts, and for a 0 it has 0volts\). These translated volts simply travel along the wire.
  * If two computers felt the same silence and sent a packet at the same time, then a "collision" occurs. When this happens, the packet is destroyed. The two computers create a random amount of time to wait, and send the packet again.
  * All computers on the network are listening and can sort of "see" the packet go by, but only receive packets addressed to them. The receiving computer simply reads this data as it comes in.
  * The sender also sends a "checksum" at the end of each packet. Which states what the total number of 1s should have been in the packet. If that number is wrong, then the receiver can request that packet be sent again.
* Wi-Fi:
  * Works the exact same, but instead of a wire you have a radio channel.

## OSI Model

* _Open Systems Interconnection Model_

### Layers

| Layer | Data unit | Examples |
| --- | --- | --- |
| 1. Physical | bit | Ethernet, USB, Wi-Fi, Bluetooth, DSL |
| 2. Data link | Frame | L2TP, PPP |
| 3. Network | Packet | **IPv4**, **IPv6** |
| 4. Transport | Segment \(Datagram\) | **TCP**, **UDP** |
| 5. Session | Data | HTTP, FTP, SMTP, DNS, IMAP, SSH, TLS |
| 6. Presentation | Data | ASCII, JPEG |
| 7. Application | Data | Chrome, Mail.app |

### Data Units

* Frame
  * Structure: Frame header \(e.g. Ethernet\) + \(Network header + Transport header + data\) + Frame footer
  * Source and destination MAC addresses, length, checksum
* Packet
  * Structure: Network header \(e.g. IP\) + \(Transport header + data\)
  * Version \(IPv4/IPv6\), source and destination IP addresses, length flags, TTL, protocol, checksum
* Segment
  * Structure: Transport header \(e.g. TCP\) + data
  * Source and destination ports, sequence number, ack number, data offsets, flags, checksum

## IP

* _Internet Protocol_
* No concept of connection.
* Every computer connected to the internet \(via its network interface card \(NIC\)\) has an IP address comprised of 4 bytes \(for v4\). \(A number between 0 and 255\). The left part of the address generally indicates the neighborhood.
* Packets are passed from one computer to the next, until reaching the destination.
* No delivery guarantee, no receiving ack.
* Sometimes multiple copies of the same packet are passes, taking different paths, and thus arriving at different times.
* Designed to be able to route around connectivity problems.
* A **router** is a computer that can be connected to multiple networks. So it can take packets from your LAN computers and send them off to another router, and another, and another, until it reaches a core router for the neighborhood. Then it passes it to the core router of the destination IP's neighborhood. And back down a chain until it reaches the destination IP address. You can run a traceroute on a domain or IP to see steps your packets take to get to a destination.

## TCP

* _Transmission Control Protocol_
* Built on-top of IP.
* Connection-based.
* Once a connection has been made between two parties, sending data between the two is much like writing to a file on one side and reading from a file on the other.
* Reliable and ordered, i.e., arrival and ordering are guaranteed.
* Takes care of splitting your data info packets and sending those across the network, so you can write bytes as a stream of data.
* Makes sure it doesn't send data too fast for the Internet connection to handle \(flow control\).
* Hides all complexities of packets and unreliability.
* Sends an ack for every packet received.
* Queues up data until there's enough to send as a packet.
* TCP tends to induce packet loss for UDP packets whenever they share a bottleneck node \(same LAN/WAN\).
* Pathway:
  * You press enter on a browser
  * Browser parses the url with the protocol, the domain, port, path, etc. \(parameters \(?id=\), anchors \(\#section\)\) \(Default port 80 for Http\)
  * Browser performs DNS lookup to attain the IP of the destination server.
  * Browser opens a socket to begin the game of hot potato between routers. TCP \(destination port added to header. 3 way handshake initiated\) -&gt; IP \(ip of destination server and of current machine added to the packet\) -&gt; Link layer \(machine address added to the packet\).
  * Browser passes packets to the modem, which converts the binary into a transmittable \(analog\) form. Then the packets are passed off to the next router toward the destination, until it reaches it.
  * Destination server will send a response back based on its own settings \(was the file found, etc.\) and which protocol was used.
  * Browser receives this response and does something with them.
    * Parses the HTML, CSS, and JS.
    * Construct DOM Tree -&gt; Render the Tree -&gt; Layout of Render Tree -&gt; Paints the Tree

## UDP

* _User Datagram Protocol_
* Built on-top of IP, very thin layer over it.
* Unreliable protocol, usually around 1-5% packet loss.
* No guarantee of ordering.
* Minimizes transmission delay.
* Send a packet to destination IP address and port; the packet will get passed from computer to computer and arrive at destination or get lost.
* Receiver simply listens on specific port and gets notified when a packet arrives, with the sender address:port, and packet size.
* One guarantee over IP – a packet will either arrive as a whole \(all of it\) at destination or not at all \(no partial delivery\).
* You need to manually break your data up into packets and send them.
* You need to make sure you don't send data too fast for your Internet connection to handle.
* Good for when you want data to get as quickly as possible from client to server without having to wait for lost data to be resent, usually real-time data.
* Examples: real-time gaming, metrics reporting, video/audio streaming.

## Browsers

* Event loop runs continuously to call methods once the stack is clear.
* Render loop runs continuously to update the DOM once the stack is clear.
* Garbage collection done mostly by "mark and sweep" algorithm these days, which states that an object is not needed anymore when it is "unreachable". Rather than reference counting which checks whether an object is no longer referenced. Reference counting lead to memory leaks with "circular referencing". With Mark and sweep that isn't an issue. Starts from global object \(the "root"\), everything that is unreachable from the root is collected.



