# The TCP/IP Protocol Suite

## Timeline

| Date        | Event                                             | Comments                                                                                               |
| ----------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| 1957, Oct 4 | Sputnik 1 is launched by the USSR.                |                                                                                                        |
| 1958, Feb 7 | ARPA is created by President Eisenhower.          | **A**dvanced **R**esearch **P**rojects **A**gency, today known as _DARPA_                              |
| 1966        | ARPANET project is initiated.                     | Distributed, packet switching, wide area network. Background: a WAN that can survive a nuclear attack. |
| 1969        | First computers connected.                        | UCLA, SRI (Stanford), UCSB, U of U                                                                     |
| 1975        | ARPANET is declared "operational".                |                                                                                                        |
| 1983        | TCP/IP becomes the standard protocol stack.       |                                                                                                        |
| 1983-1989   | Other WANs are connected to ARPANET using TCP/IP. |                                                                                                        |
| 1990        | ARPANET is decommissioned.                        |                                                                                                        |
| 1994        | The Internet is opened to the general public.     |                                                                                                        |

![PDP-10](/images/DECSystem10-KI10.jpg)

![ARPANET](/images/Arpanet_map_1973.jpg)

![PDP-11](/images/Pdp-11-40.jpg)

![ARPANET](/images/Arpanet_logical_map,_march_1977.png)

![TCP/IP Internet Map](/images/InetCirca85.jpg)

-   A _network of networks_, made up of many **L**ocal **A**rea **N**etworks, like the ones in your home or office building, and **W**ide **A**rea **N**etworks.

## Overview

The Internet consists of 4 layers which are numbered from the bottom up:

| Layer # | Layer Name  | Provides                                                      | Implemented By                                               | Protocols                                                                                                        | Related Concepts                                          |
| ------- | ----------- | ------------------------------------------------------------- | ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| 4       | Application | A specific service, e.g. the world-wide web.                  | Applications (e.g. web servers and browsers), code libraries | HTTP &bull; Every application-specific protocol, e.g. email (POP3 &bull; SMTP &bull; IMAP), WhatsApp, Netflix... | Domain Names, DNS &bull; Request/Response &bull; URL      |
| 3       | Transport   | Process-to-process communication.                             | OS                                                           | TCP &bull; UDP                                                                                                   | Port &bull; Socket &bull; Connection &bull; Client/Server |
| 2       | Internet    | Global host-to-host communication.                            | OS                                                           | IP                                                                                                               | IP Address &bull; IPv4/IPv6                               |
| 1       | Link        | Host-to-host communication over a single network (LAN or WAN) | Hardware, OS                                                 | WiFi &bull; Ethernet &bull; ADSL &bull; Cellular Data (LTE, 4G, 5G...)                                           | Physical (MAC) Address                                    |

## The Link Layer

![Submarine Cable Map](/images/submarinecablemap.png)

[submarinecablemap.com](https://submarinecablemap.com)

-   Allows direct communication between computers that are connected by a _physical medium_ (wire, optical fiber, radio frequency, satellite link...) to each other or to a common router.
-   LANs usually use _Ethernet_ cables and/or _WiFi_ to connect end devices to a central router.
-   End devices have a 48-bit _physical, or MAC, address_.
-   To send a message from one computer to another on the same LAN, the sender must know the recipient's physical address.

![Pneumatic Tube Hub](/images/pneumatic-tube-hub-1900s.jpg)

![Pneumatic Tube Hub](/images/pneumatic-tube-hub-1940s.jpg)

![Pneumatic Canister](/images/Pneumatic-Tube-New-York-City-Postal-Service-Mail.jpg)

-   Pneumatic tube analogy: to send a package, you need to
    1.  Put the package in a canister.
        1.  Write the recipient's name on one side of the canister.
        1.  Write the sender's name (yours) on the other side.
    1.  Put the canister in your end of the pneumatic tube and send it to the sorting room.
    1.  A worker in the sorting room receives the canister, reads who is supposed to receive it, puts it in the proper tube and sends it to that person's room.
    1.  The recipient opens the canister and takes the package.
-   Similarly, to send digital information to another computer on the same LAN, the link layer
    1.  Constructs a _frame_, made up of
        1.  The _frame header_ (14 bytes), containing the physical addresses of the sender and the recipient.
        1.  The actual data, or _payload_.
        1.  The _frame footer_ (4 bytes).
    1.  Transmits the frame over the physical medium (wire or radio frequency) connecting your device to the router.
    1.  The router repeats the frame over the medium that connects it to the recipient.
    1.  The other computer extracts the payload from the frame and interprets it somehow.

## The Internet Layer

-   Allows communications between _any two_ computers on the Internet, not just those that are directly connected to each other.
-   Consists of one main protocol, the _Internet Protocol_ (IP).
-   IP Addresses:

    | Version | Bits | Example                             |
    | ------- | ---- | ----------------------------------- |
    | IPv4    | 32   | `10.83.237.32`                      |
    | IPv6    | 128  | `2a03:2880:f22d:c5:face:b00c:0:167` |

-   Uses other computers on the Internet as _routers_, or _gateways_ (i.e. relays) between different networks.
-   IP software is built into the operating systems of the end computers and of each intermediate router.
-   To send a message to another computer on the Internet:

    1. Construct a _packet_, made up of
        1. The _IP header_, containing the IP addresses of the sender and recipient.
        1. The actual data, or _payload_.

    ![By en:User:Cburnett original work, colorization by en:User:Kbrose - Original artwork by en:User:Cburnett, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=1546338](/images/TCP_encapsulation.svg)

    1. If the destination address belongs to the local network:
        1. Send the packet, using the link layer, directly to the destination.
    1. If the destination address does not belong to the local network:
        1. Use a _routing table_ to decide which gateway on the local network is closest to the packet's intended destination.
        1. Use the link layer to send the packet to that gateway (e.g. your DSL router).
        1. The gateway relays the packet to the other network.
    1. Repeat steps 2-3 until the packet reaches its destination.
    1. At the destination, extract the payload from the packet.

-   So, an Ethernet frame contains an IP packet, which, in turn, contains the payload.

![en:User:Cburnett original work, colorization by en:User:Kbrose, CC BY-SA 3.0 <http://creativecommons.org/licenses/by-sa/3.0/>, via Wikimedia Commons](/images/IP_stack_connections.svg)

![Matryoshka](/images/matryoshka.jpeg)

#### The Loopback Interface

-   The IP address `127.0.0.1` is special: any packet sent to this address comes back as an incoming packet without actually being transmitted to the network.
-   The name `localhost` can usually be used as an alias for this address.
-   This allows inter-process communication between processes on the same computer using the same code as for networked communication. _Extremely useful during development!_

#### DNS

-   Servers are usually referred to by a _domain name_ rather than an IP address.
-   The Domain Name System (DNS) is a _hierarchical_, _decentralized_ directory service that translates domain names to IP addresses.

## The Transport Layer

**Question:** So, the Internet layer allows us to communicate with any computer on the Internet. Why do we need more layers, then?

**Answer:** The Internet Layer provides _host-to-host_ communication, but we need _process-to-process_ communication.

#### Processes

-   Modern operating systems support _multitasking_: running more than one program at the same time.
-   A _process_ is an isolated execution environment in which the OS runs each program.
    -   You can think of a process as a kind of "bubble" that the OS inflates around the application's code and data, isolating them from the rest of the software running on the computer at the same time.
-   The process gives the application code the illusion that it the only program running on the computer.
-   An application running inside a process can't accidentally damage another application by changing its memory.
-   If an application crashes, it doesn't crash the whole system.

#### Processes and IP Packets

-   Incoming IP packets are received by the OS.
-   However, most packets are meant to be handled by some specific application. For example, an HTTP response should go to the browser that requested it.
-   If we let each running process pick the packets it wants to handle, bad things can happen.
-   So, the OS needs a way to determine which process should get each packet.

#### Ports and Sockets

-   A _port_ is simply a 16-bit integer (a whole number between 0 and 2<sup>16</sup> = 65536, exclusive).
-   The transport layer adds two port numbers to each packet: one for the sender and one for the receiver (in the purple segment in this diagram).

![Packet Encapsulation](/images/TCP_encapsulation.svg)

-   Processes use _sockets_ to receive packets with specific port numbers.
-   You can think of a socket as a physical socket in the wall of the bubble that represents the process.
    ```mermaid
    flowchart BT;
        subgraph Server Host
            subgraph LR os2[OS]
                link2[Link Layer] --> ip2[Internet Layer] --> transport2[Transport Layer]
                transport2 --> ip2 --> link2
            end
            subgraph Server Process
                transport2 <--> socket2[Socket<br/>TCP/80]
                app2[Application Code] <--> socket2
            end
        end
        %% wire((Physical Medium))
        link1 <--> link2
        subgraph RL host1[Client Host]
            subgraph BT proc1[Client Process]
                app1[Application Code] <--> socket1
            end
            subgraph OS
                socket1[Socket<br/>TCP/64206] --> transport1[Transport Layer] --> ip1[Internet Layer] --> link1[Link Layer]
                link1 --> ip1 --> transport1 --> socket1
            end
        end
        %% link1 <--> medium((Physical Medium)) <--> link2
    ```
-   Generally speaking, two sockets on the same computer cannot be bound to the same port number. Attempting to bind to an already bound port results in a fatal error, usually terminating the process.
-   The OS uses the receiving port in the transport header to determine which socket should receive the packet.
-   The client asks the OS to assign to its socket an _ephemeral_ (temporary) port in the range 49152-65535.
-   The server binds to a _well-known_ port number. For example:

    | Service              | Protocol | Default Port |
    | -------------------- | -------- | ------------ |
    | Domain Name (DNS)    | UDP, TCP | 53           |
    | File Transfer (FTP)  | TCP      | 21, 20       |
    | Mail (SMTP)          | TCP      | 25           |
    | Mail (POP3)          | TCP      | 110          |
    | Unsecured web (HTTP) | TCP      | 80           |
    | Secure web (HTTPS)   | TCP      | 443          |
    | MySQL                | TCP      | 3306         |
    | MongoDB              | TCP      | 27017        |

-   Port numbers below 1024 are assigned by [IANA](http://iana.org).

#### UDP (User Datagram Protocol)

-   UDP is a very simple protocol that mainly adds the two port numbers (sender and recipient) to the underlying IP packet and not much else.
-   UDP communication uses discrete _datagrams_ (messages) like letters in the mail.
    -   Each datagram is delivered in one IP packet.
-   UDP is useful for real-time applications like live video and audio and multiplayer games.

#### TCP (Transmission Control Protocol)

-   Much more sophisticated than UDP.
-   Allows application code to view the data as one long _stream_, or series, of bytes (actually two streams: one in each direction).
-   Connection-based, like a telephone call.
-   Doesn't limit message size.
-   Used as the transport layer for HTTP.

#### Comparison of UDP and TCP

|                  | UDP                                        | TCP                                                     |
| ---------------- | ------------------------------------------ | ------------------------------------------------------- |
| **Data Model**   | Discrete datagrams                         | Continuous byte stream                                  |
| **Metaphor**     | Mail correspondence                        | Telephone call                                          |
| **Connection**   | Connectionless                             | Connection-based                                        |
| **Reliability**  | Unreliable (no ACK)                        | Reliable (requires ACK)                                 |
| **Delivery**     | Immediate                                  | May be delayed due to retransmission                    |
| **Packet Order** | Not guaranteed                             | Guaranteed to be the same order in which they were sent |
| **Useful For**   | Real-time: live audio, video, online games | Everything else                                         |

#### TCP Sockets

-   A TCP socket can be in one of several states, including:
    -   `CLOSED`
    -   `LISTEN`
    -   `ESTABLISHED`

#### TCP Socket Life Cycle

-   The server creates a socket and _binds_ it to the well-known port number.
-   The server puts the socket in the `LISTEN` state (this is sometimes referred to as a "passive open") and starts waiting for a client to connect.
-   The client creates its own socket and tells it to connect to the server using the server's well-known IP address and port number.
-   When the TCP connection is established, instead of changing the state of the socket from `LISTEN` to `ESTABLISHED`, the OS creates _a new socket_ in the `ESTABLISHED` state and binds it to all five connection parameters: the protocol (TCP), the server's own IP address, the client's IP address, the server's port and the client's port.
-   Usually the server spawns a new process to handle communication on the connected socket.
-   The original socket stays in the `LISTEN` state and can accept a connection from another client.

```mermaid
sequenceDiagram
    %% participant user as User
    participant client as Browser
    participant csocket as Client Socket
    participant ssocket2 as Connected <br/> Server Socket
    participant server2 as Sub-Process
    participant ssocket as Server Socket
    participant server as Web Server

    server->>ssocket: bind(*:80)
    Note over ssocket: CLOSED
    server->>ssocket: listen()
    Note over ssocket: LISTEN

    loop
        server->>+ssocket: accept()
        Note over csocket: CLOSED
        %% user->>client: (Search Google for "tcp socket")
        client->>+csocket: connect(www.google.com:80)
        csocket->>csocket: bind(<client host>:52428)
        Note over csocket,ssocket: TCP Handshake
        csocket->>ssocket: SYN
        ssocket->>csocket: SYN+ACK
        csocket->>ssocket: ACK
        Note over csocket: ESTABLISHED
        csocket-->>-client: ...
        ssocket-->>-server: Connected<br/>Socket
        par Handle client request
            Note over ssocket2: ESTABLISHED
            loop
                server2->>+ssocket2: recv()
                client->>+csocket: send("GET /?q=tcp%20socket")
                csocket->>ssocket2: GET /?q=tcp%20socket
                csocket-->>-client: ...
                client->>+csocket: recv()
                ssocket2-->>-server2: "GET /?q=tcp%20socket"
                server2->>+ssocket2: send("200 OK...")
                ssocket2->>csocket: 200 OK <br/> <html>...</html>
                ssocket2-->>-server2: ...
                csocket-->>-client: "200 OK..."
            end
            client->>+csocket: close()
            csocket->>ssocket2: FIN
            ssocket2->>csocket: ACK
            Note over csocket: CLOSED
            ssocket2->>csocket: FIN
            csocket->>ssocket2: ACK
            Note over ssocket2: CLOSED
            csocket-->>-client: ...
        end
    end
```
