---
title: 'How to Use Node.js to SSH into Remote Servers: A Comprehensive Guide'
date: '2023-7-02'
tags: ['javascript', 'node', 'ssh']
draft: false
summary:
images: [/static/images/ssh.jpg]
---

# How to Use Node.js to SSH into Remote Servers: A Comprehensive Guide

Secure Shell (SSH) is a powerful protocol that allows secure remote access to servers and secure communication between systems. If you are a developer you might need to install, configure or execute some commands on a server that might not be available to you on your local environment. SSH is one such tool that allows you to securely connect to and control a remote server from your local machine. Today in this article we’ll guide you to build a Node.js application that will ssh over a remote server to connect and execute commands on it.

<img src="/static/images/ssh.jpg" />

## Prequrities

- Node.js running on your machine
- Remote server
- Basic of CLI

## What is SSH?

SSH is a network protocol that allows you to securely connect to a remote server and execute commands on it. SSH is a very popular protocol, and it is used by many different applications.SSH is based on the client-server model, where a client is an application on the local system, and the server is the host you wish to connect to. The client application takes the remote host information, such as username, password, etc., as input and establishes an encrypted SSH session with the remote host.

In the next section, you will learn what are the uses of SSH.

## Uses of SSH

Before building the application node SSH application let’s know why we are using SSH:

- Logging into a remote server
- Executing commands on a remote server
- Transferring files between a client and a server
- Managing servers
- Troubleshooting problems

## Using SSH

If you're using a UNIX-based operating system like Ubuntu or macOS, you can easily access SSH as it usually comes pre-installed.

To establish an SSH connection, follow these simple steps:

1. Open the terminal.

2. Determine the hostname or IP address: You need to know the hostname or IP address of the remote system you want to connect to. The hostname is the name of the machine, and the IP address is a numeric identifier for the machine. You can typically find the hostname by typing the command `hostname` in the terminal.

3. Formulate the SSH command: In the terminal, type the following command:

```

ssh <username>@<hostname or host IP>

```

Replace `<username>` with the username of the remote machine you want to connect to. Replace `<hostname or host IP>` with either the hostname or the IP address of the remote system. For example:

```

ssh ubuntu@219.123.101.201

```

This command establishes an SSH connection with the host whose IP address is 219.123.101.201, using the username "ubuntu".

4. Connected via SSH: If the provided details are correct, you will be successfully connected to the remote system via SSH. You can now execute commands, transfer files, or perform other operations on the remote machine.

By following these steps, you can establish an SSH connection to a remote system and securely manage it from your local machine. SSH provides a secure way to access and control remote systems, making it an essential tool for system administrators, developers, and anyone who needs remote access to servers or machines.

## Using ssh in nodejs

Node.js provide many packages that implement the SSH client and server capabilities. Some of them are node-ssh, simple-ssh, ssh2, etc. In this application, we are using ssh2 as this is the most popular among others. With this, you can execute commands on a remote server and have your Node application perform and automate your tasks for you.

### Setting up ssh in node

To set up a Nodejs project you have to run:

```

npm init -y

```

The above command will initialize the project as an NPM project and the -y flag accepts the command with all the default values. At this point, you will have a package.json file at the root of the project.

Install ssh2 by running the following command

```

npm install ssh2

```

SSH2 is a client and server module which is written in pure JavaScript for node.js.

After installing the required dependencies you will need access to a remote server. For this particular application, I’ve created an EC2 server and downloaded a private key to my local machine.

Let's walk through the code to establish secure connections through the node.js application.

```

const Client = require('ssh2').Client;

// Create a new SSH client instance
const sshClient = new Client();

// Configure the connection parameters
const connectionParams = {
  host: 'your-ssh-host',
  username: 'your-username',
  privateKey: require('fs').readFileSync('/path/to/your/private-key')
};

// Connect to the SSH server
sshClient.connect(connectionParams);



```

By executing this code, you will now initiate an SSH connection to the specified server using the provided connection parameters. You can then proceed to perform various SSH-related operations, such as executing remote commands, transferring files, or handling events, using the sshClient instance.

### Establishing ssh in node

Now that we have set up the SSH library and configured the connection parameters, let's establish an SSH connection programmatically using Node.js

```

sshClient.connect(connectionParams);

// Handle events when the connection is established

sshClient.on('ready', () => {

    console.log('Connected via SSH!');

// Now you can execute commands, transfer files, etc.

});

// Handle errors during the SSH connection process

sshClient.on('error', (err) => {

    console.error('Error connecting via SSH:', err);

});


```

After you provide the connection parameter and run your above application you will receive the below output which indicates that your application has been connected to a remote server. If it failed to connect to the remote server it will fire an error event on sshClient and log to the console with the reason.

![](https://lh3.googleusercontent.com/Lb0f-XGBrWC4V8ioGjeOZjfmrhcf5oIOkxfVsQ9_5iCFSljojzFpSrKh515EtUp6wBkYOIC8V9-6i1sCC_ZM3IMqDW1MyHkOrh5ZnCZZB0IyFIFq9EWQDAp1vzxynUT7qgP04MZ-AoxJ-AJJ3pirXMo)

### Executing a command in ssh

With the SSH connection established, we can execute commands on the remote server using Node.js. Let's see an example where the application will take a command from user input and execute it on the server and retrieve a response.

```

function execute() {
  // Prompt the user to enter a command
  rl.question('Enter a command to execute on the remote server: ', (command) => {
    // Execute the user-entered command on the remote server
    sshClient.exec(command, (err, stream) => {
      if (err) throw err;

      stream
        .on('close', (code, signal) => {
          console.log('Command execution closed');
          sshClient.end();
          rl.close();
        })
        .on('data', (data) => {
          console.log('Command output:', data.toString());
        })
        .stderr.on('data', (data) => {
          console.error('Command error:', data.toString());
        });
    });
  });
}

```

The readline library allows users to provide input using the command line. This function will be executed after the SSH connection has been established which will ask for input command to execute on a server.

Let’s try executing the uptime command on the server which will return the uptime of the server.

![](https://lh5.googleusercontent.com/AVCO4YDrdrgC3WuexQApdWjA8n_v2HS58-0j6oAqQpAzQPcC8ISq06wel9c7X5d59rqDGPFhd8mjek_7mfJHt6JSqKmHChfsUIzikObVgYElKQgP9a8LXcVP3Kfe1Q6LJc1_DQvvBsGguVDMqy1Zp_o)

### Conclusion

In this blog post, we showed you how to use Node.js to SSH into remote servers. We covered the following topics:

- What is SSH?

- How to install the necessary Node.js modules

- How to create a simple SSH connection

- How to execute commands on the remote server

- How to handle errors

We also provided some examples of how you can use Node.js to SSH into remote servers in your own applications. You can find the code used in this application in [git repository](https://github.com/bipinparajuli/nodejs-ssh)
