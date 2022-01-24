---
title: "OSI Model"
date: "2022-01-24"
---


# What is OSI model?

Open System Interconnection model or OSI is a seven-layer model used to describe networking connections. OSI models defines how information is transmitted from source to its destination through a physical medium and how it interacts with the application. it describes the functions of a networking system.


# The OSI model

The OSI  divides a communication system into seven abstraction layers.

The layers are:

 * Application layer

 * Presentation layer

 * Session layer

 * Transport layer

 * Network layer

 * Data Link layer

 * Physical layer


Let's start understanding the OSI model.

 1. Physical Layer:

    Physical Layer represents the electrical and physical representation of the data connection/system. Physical layer is responsible for the actual physical connection between the devices. It takes care of the transmission of raw bit streams.

    It includes the physical resources like cables, modems, network adapters, and hubs, etc.


 2. Data Link Layer:

    The data link layer is responsible for the node to node delivery of the message. The main fuction of this layer is to make sure data transfer is error-free from one node to another, over the physical layer.

    Data Link layer is IP address understandable layer, which helps us to define logical addressing so that any endpoint should be identified.

    This layer helps us to implement routing of packets through a network.

 3. Network Layer

    Network layer works for the transmission of data from one host to the other located in different networks. It also takes care of packet routing. Ip address of sender and reciver's are placed in the headier by the network layer. Segment in Network layer is referred as Packet.

    The main function of Network layer are:

    * Routing
    * Logical Addressing

4.  Transport Layer

    The Transport Layer deals with the coordination of the data transfer between end systems and hosts. How much data to send, at what rate, where it goes, etc. The data in the transport layer is referred to as Segments. It gets the data from the session layer and breaks it into transportable segments.

    Example of the Transport Layer are the UDP(User Datagram Protocol) and TCP(Transmission Control Protocol) that is build on top of the Internet Protocol (IP model), which work at layer 3.


 5. Session Layer

    This layer is responsible for establishment of connection, maintenance of sessions, authentication and also ensures security. This layer is responsible for opening sessions and ensuring that they're functional during data transfer.

    Important function of Session Layer are:

    * It establishes, maintain and ends a session.

    * Session layer enables two systems to enter into a dialog

    * It also allows a process to add a checkpoint to stream of data.

6. Presentation Layer

    The presentation layer is responsible for ensuring that the data is undertandable for the end system or useful for later stages. It translates or formats data based on the applications's syntax or semantics. The presentation layer translates data from the network format to the application format or vice versa.

    The functions of the presentation layer are:

    * Translation: For example, ASCII to EBCDIC.

    * Encryption/Decryption: Data encryption transates the data into another form or code.

    * Compression: Reduces the number of bits that need to be transmitted on the network.


 7. Application Layer

    At this layer user direclty interacts with a software application, so it is closest to the end user. When the user wants to transmit files or pictures, this layer intreacts with the application communicating with the network.

    The functions of the Application layer are:

    * Network Virtual Terminal

    * Mail Services

    * Directory Services

    Some common protocols incude:

    * SMTP or POP3 for emails

    * FTP for emails

    * Telnet for controlling remote devicess









