---
title: Create UDP Client And Server With Node Js
date: '2022-09-23'
tags: ['Software Development', 'Backend', 'Network', 'Node']
draft: false
summary:
images: []
---

## Create UDP client and server with Node JS

Node.js uses [dgram](https://nodejs.org/api/dgram.html) library to implement the UDP datagram socket. In a computer network, UDP stands for User Datagram Protocol. This Protocol allows the computer application to send messages via datagrams from one machine to another over the Internet Protocol (IP) network.

![](https://lh5.googleusercontent.com/X-01YQE3XS3G2PsJyzjS2hy7rW86d9ZO88N9zm3MQ2XxFh8upP4m7IVU9-OkOJ1J6eNrEyPX6Oz7BTSod2aLXzC8ki48CT7i965X8BThD4a99M7D2YU0eYmeMHHQp9F35gbDuzk0iiJXJkwoyYEllZ3mpN_r2cAWstQU1GwS9eWC8BNspc8OSKbcvQ)

## Table of content

- Prerequisites

- Introduction to UDP
- Compare UDP with TCP
- Why UDP
- How UDP work
- Node js introduction
- Node js and project setup in local environment

- Building UDP application

  - Building the UDP Client

  - Server

- Running client and server

- Conclusion

## Prerequisites

Before going into the further details of UDP it’s expected that you have the following prerequisites:

- Node js running on a machine.
- Good to have a basic idea of TCP/IP protocol.
- Some knowledge of how to work with Node.js is essential.
- How to use node modules would be very helpful as we are using the modules in this blog.

The code examples are available on a public [Github repository](https://github.com/bipinparajuli/Nodejs-udp) for your reference. You can also check out [OSI Reference Model](https://bipinparajuli.com.np/blog/osi-model) to have a basic understanding of the OSI Model. We’ll be starting through the basic introduction.

## Introduction to UDP

The User Datagram Protocol, or UDP, is a communication protocol used across the internet for especially time-sensitive transmissions like video streaming. UDP protocol is a connectionless protocol specially designed for the speed of data transmission. UDP provides no acknowledgment mechanism, which means that the receiver does not send the acknowledgment for the received packet, and the sender also does not wait for the acknowledgment for the packet that it has sent.

## Comparison between UDP AND TCP

UDP is an alternative communication protocol to TCP protocol(transmission control protocol). Both the TCP and UDP protocols send the data over the internet protocol network, so it is also known as TCP/IP and UDP/IP. UDP sends the messages in the form of datagrams, it is considered the best-effort mode of communication. TCP sends individual packets, so it is a reliable transport medium. Another difference is that the TCP is a connection-oriented protocol, and the UDP is a connectionless protocol as it does not require any virtual circuit to transfer the data.

## Why UDP

As we know that UDP is an unreliable protocol, but still we require a UDP protocol in some cases. The UDP is deployed where the packets require a large amount of bandwidth along with the actual data. For example, in video streaming, acknowledging thousands of packets is troublesome and wastes a lot of bandwidth. In the case of video streaming, the loss of some packets couldn’t create a problem, and it can also be ignored.

## How UDP Work

UDP transfers data between two computers in a network in a simple fashion in comparison to other protocols. It sends packets directly to the target computer, without establishing a connection first, indicating the order of said packets, or checking whether they arrived as intended.

The step-by-step processes to send packets over UDP are listed below:

- Data are gathered in one or more UDP packets.
- The header information including the source and destination ports to communicate with, the packet length, and a checksum is added to each datagram.
- The datagrams are encapsulated in an IP datagram.
- The datagrams are sent off to their destination

![](https://lh5.googleusercontent.com/nhOsydWPQx4tR2yTeSjj9deCrQ0g8Irq68_m68dpGm93_oUYtwZnsxduhgnyE78Ue1ueoTZKhMqoB6c-NnhoBiBT2BAoydOOknYJRDruGCigeVY4f4hJGIP8q4f7MawvCWYfUU9kCLQz6u7GZGyLga-2Bry93I2F9d32Qys-RzhaM6c3Kl80pk9zAw)

## Introduction to Node.Js

[Node. js](https://nodejs.org/en/about/) is an open-source, cross-platform, back-end JavaScript runtime environment that runs on a JavaScript Engine and executes JavaScript code outside a web browser, which was designed to build scalable network applications.

We’ll be using Node.js to build the UDP server and Client in this blog. If you are looking for getting started with Node.js you could follow through with the getting started [guide](https://nodejs.org/en/docs/guides/getting-started-guide/).

## Project Setup

Now, let’s first check the node version to get started with creating a UDP application. Open a terminal or command prompt on Windows and run the following command to check the Node version:

```js
node - v
```

or

```js

node --version

```

If you see the result of your node version you are good to go else you could follow the [link](https://nodejs.org/en/download/) to setup Node on your machine

I will be creating two files server.js and client.js inside one directory for ease.

![](https://lh5.googleusercontent.com/J2ZGzY8HD3_R87MEC6LtCjRPrBvfkFWJ_Zf3TmQXqoNKVUAJk9-M2B1Zvo85f_9GDcaQjl__wvRe_YwlLycpAK70V2L9Arm76Rplp6lDKuiTu7dgURIcVaiBl11Kb9_xzUVz47F5thAN62BSoYKIkz-EqwOIDvd2hMnHfFbsej1GN91Rt7QSSYcqtA)

## Building UDP Application

In this tutorial, I will be creating a simple UDP client/server application. The server will be listening on a specific port in which the client messages will be received and will send a response to the client. And the client will send the server a message and wait for a response to its request.

### Building the UDP Client

UDP clients provide a simple method for sending and receiving connectionless UDP datagrams in blocking synchronous mode. Because UDP is a connectionless transport protocol, you do not need to establish a remote host connection prior to sending and receiving data. Node.js use [dgram](https://nodejs.org/api/dgram.html) library to implement the UDP datagram socket. It’s nice to have a look. The following code snippet sends a datagram packet to a server specified by a port on our local machine and receives a packet.

```js
const UDP = require('dgram')

const client = UDP.createSocket('udp4')

const port = 2222

const hostname = 'localhost'

client.on('message', (message, info) => {
  // get the information about server address, port, and size of packet received.

  console.log('Address: ', info.address, 'Port: ', info.port, 'Size: ', info.size)

  //read message from server

  console.log('Message from server', message.toString())
})

const packet = Buffer.from('This is a message from client')

client.send(packet, port, hostname, (err) => {
  if (err) {
    console.error('Failed to send packet !!')
  } else {
    console.log('Packet send !!')
  }
})
```

As you can see, once the socket is opened, receiving a packet is very simple. And the code converts the Buffer array to string using the toString method. Info gives information about the source of packets.

### UDP Server

The following code demonstrates how to implement the server for the above client. Now we’ll be creating a UDP server listening on port 2222 and waiting for the client’s request:

```js
const UDP = require('dgram')

const server = UDP.createSocket('udp4')

const port = 2222

server.on('listening', () => {
  // Server address it’s using to listen

  const address = server.address()

  console.log('Listining to ', 'Address: ', address.address, 'Port: ', address.port)
})

server.on('message', (message, info) => {
  console.log('Message', message.toString())

  const response = Buffer.from('Message Received')

  //sending back response to client

  server.send(response, info.port, info.address, (err) => {
    if (err) {
      console.error('Failed to send response !!')
    } else {
      console.log('Response send Successfully')
    }
  })
})

server.bind(port)
```

As you see, the server uses the client’s address and port to send the DatagramPacket that is obtained from the client DatagramPacket received from the client previously.

## Running Client and Server

As I have created the client and server in different files, we need to run them separately. Let’s start by running the server first. Enter the following command on your terminal

```js

node server.js

```

Once the server is up and running you will see the output as our server is listening on the following port and address.

![](https://lh4.googleusercontent.com/8DByk9QNgFC85WNYGA0p5c-SR9oI1TBEdCDGd49GmqSJe83BloPKqJgb_hWArosm2HVCRHmzDPgqC2Fe5q1hCuAnet-PG2MBWZUaxjFTspd5pQpwm3d0gYbRPjSMpowNoxie4QQNIE6cHsPm5w4TeoH2Lp-C34JOcx6GByQ5Mra3wec3J01rJQwNEA)

Now we can lunch our client to send packets to the listening server. Running the client will give you the following result:

![](https://lh6.googleusercontent.com/gSHcW2SkfMj_Ak5JpPvDmiCmFsTvdi60e1mOwYh6Zq0N-khIedC3kKFXE3j8C-3DrXSpTfqdMwR5-RQp84uv0zdUnClBmPhSqgLJOVzO1wiHjIVsJ_fcc75-j7WESxFSON_xpwMUJCIlAlPG83uiZdZy4fabQXQB2UFT-YdosMrP3T9nDT_2p79eVA)

As we see on the result the packets are sent to the server as the server receives the packet it will respond with a Message Received to our client as displayed above. If we see the server output once again we can see the packet send from sent converted to string on our server.

![](https://lh5.googleusercontent.com/NrMREuGFXcQZ5KcvBzVZAEgWGJ9Ncliji8vrIIewFEz9sIuihitdu5TfZMDxoagBMSnZNpPGhuuCDz6sjtK4iWg06dkY3s9sqQ9gcubXwTbCat22U2Wdk1Ma7U_UJDsP-P2GA4KnYQGvXFWMCjBODGIkjLI49DYHh7_qD4Wyocs24-HtKwqA2pOvCA)

## Conclusion

We looked at how to create a UDP client and server with Nodejs. I hope you have learned how to create simple UDP applications in node.js.
