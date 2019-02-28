# Heartbleed Bug

## Description of the scenario

The Heartbleed bug is an example of a cybersecurity attack that exploits a vulnerability in the OpenSSL library. Briefly, 
a missing validation step in the OpenSSL library could allow a hacker to access sensitive information on a server that is 
using the vulnerable library. As part of the handshake protocol for establishing a SSL connection between a client and 
server, a *heartbeat* message is sent from the client, which is then relayed back from the server. The client is also 
responsible for sending the length of its heartbeat message, which the server uses to determine the number of bytes 
from memory that need to be returned. A spurious length value, which isn't validated by the server can cause it to 
return bytes adjacent to the memory location where the client's heartbeat message is stored.

This repository contains a demonstration of how the Heartbleed bug can be exploited to extract session cookies from a 
webserver; allowing a hacker to access the secure, access-controlled portion of a website.

## Target Audience

### Instructors

If you are an instructor teaching cybersecurity concepts and secure coding, you can use this example to provide hands-on 
experience with the Heartbleed bug and demonstrate the ease with which privileged information can be retrieved. 

### Students

If you are a student in a cybersecurity class, or, a budding programmer, it is instructive to understand how minor mistakes 
in code can lead to large security vulnerabilities. If you are currently learning about the Heartbleed attack in your class, 
you can get further practical experience with how these attacks operate and what can be done to prevent such attacks.

## Design and Architecture

This demonstration is designed using three Docker containers, one each for the server, hacker, and, victim. A simple Python Flask 
based website is hosted on the server container that returns a welcome message when logged in successfully. The server container 
runs an unpatched version of the OpenSSL library that has the Heartbleed vulnerability. Both the hacker and victim containers 
provide a VNC interface to an Ubuntu machine. The hacker container also contains a Python script that exploits the Heartbleed bug to 
send an incorrect message length for a heartbeat message, and parses the data returned from the server to look for session cookies.
Instructions are provided in a HTML document on the hacker container desktop.

## Installation and Usage

The recommended approach to running this set of containers is on CHEESEHub, a web platform for cybersecurity demonstrations. CHEESEHub 
provides the necessary resources for orchestrating and running containers on demand. In order to set up this application to be 
run on CHEESEHub, an *application specification* needs to be created that configures the Docker image to be used, memory and 
CPU requirements, and, the ports to be exposed for each of the three containers. The JSON *spec* for this Heartbleed demonstration can be 
found [here](https://github.com/rkalyanapurdue/catalog/tree/master/heartbleed).

CHEESEHub uses Kubernetes to orchestrate its application containers. You can also run this application on your own Kubernetes 
installation. For instructions on setting up a minimal Kubernetes cluster on your local machine, refer to [Minikube](https://github.com/kubernetes/minikube). 

Before being able to run on either CHEESEHub or Kubernetes, Docker images needs to be built for the three application containers. 
Container definitions for the hacker, victim, and, server can be found in the *heartbleed-hacker*, *heartbleed-victim*, and, *heartbleed-server* directories 
respectively. To build these containers, run:

```bash
cd heartbleed-hacker
docker build -t <hacker image tag of your choice> .

cd heartbleed-victim
docker build -t <victim image tag of your choice> .

cd heartbleed-server
docker build -t <server image tag of your choice> .
```

Once the Docker images have been built, you can run the containers using just the Docker engine.

```bash
docker run -d -p 80 <hacker image tag from above>

docker run -d -p 80 <victim image tag from above>

docker run -d <server image tag from above>
```

Since the user facing interface of the hacker and victim containers is the VNC interface, we expose port 80 to be accessible from the 
host machine. Since we will only access the server website from inside the hacker and victim containers, we do not bother to map any 
of the exposed ports of the server container.

### Usage
On navigating to the URL of the hacker container in your browser, you will be presented with a Linux desktop interface. Double click the 
*Instructions.html* file on the desktop to review the step-by-step instructions for following this demonstration. A new browser or terminal 
window can be launched on the hacker and victim containers by selecting the program menu at the bottom left corner. The server's IP address 
that the hacker and victim will use to access the server's hosted website can be obtained by starting a console on the server container 
from the CHEESEHub container listing page.

## How to Contribute

To report issues or contribute enhancements to this application, open a GitHub issue. 

