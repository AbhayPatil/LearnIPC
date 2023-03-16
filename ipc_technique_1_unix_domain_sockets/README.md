## Intro

This directory contains my notes and assignments from the Udemy course - [https://www.udemy.com/course/linuxipc/](https://www.udemy.com/course/linuxipc/)

## IPC Technique 1 - Unix Domain Sockets (UDS)

### Sockets Introduction

1. System-call interface is between application and OS/Kernel layers. It is also called socket layer. Some examples of system call apis are socket(), accept(), select(), etc.

### Socket Message Types

1. Connection Initiation Request (CIR) - Client process requests dedicated channel from server process.
2. Service Request Message (SRM) - Client requests some service using API provided by server.

### Socket Design

1. In unix *file descriptors* are just handles and are positive integers.
2. When server boots up it creates connection socket *(master socket file descriptor (M))* using socket(). M is only used to create data sockets. M does not handle data exchange between server and client. Server does this using *accept()* system call.
3. When new connection request comes from the client, server creates new handle (say, C1, C2, ...) for every individual client. Client handles are also called as *data sockets* or *communication file descriptors *.
4. Now clients can send SRM to server. The OS diverts the SRM from client to that client's data socket *(not master socket)* inside the server. The data sockets then send back the service response back to the clients.

### Socket *accept()* system call

1. When server recieves connection request from client on it's master socket file descriptor, accept() is invoked.
2. As a result, a client handle for this client is client is created.
3. After this whenever an SRM for this client comes to server, the OS diverts it directly to the client handle and master socket file descriptor is not involved anymore.

### UDS Introduction

1. Used in IPC between two processes on **same** machine.
2. The communcation can be **stream** or **datagram** based.

![Image](./Images/TCP_IP_socket_diagram.png)