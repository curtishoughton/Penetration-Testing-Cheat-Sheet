# Bypassing SSL Certificate Pinning Protections Multiple Ways

## What is SSL Certificate Pinning

SSL Certificate pinning is a machanism that protects against the interception of HTTPS (TLS/SSL) traffic on a mobile device. A certificate or "public hash" of an SSL certificate is embedded within the application binary. When the application initiates a connection to the target server, the certificate/hash is checked. If it does not pass verification - for example because of an attacker attempting to man-in-the-middle the traffic - the connection will be terminated and the application will not be able to connect to the web server.

To test the backend of an iOS application, which could be a web server running an API, SSL pinning needs to be bypassed, which can be carried out in a number of ways.

## Installing the Required Tools

The Frida and Objection frameworks need to be installed on both the iOS device, using Cydia, and Linux system using Python 3's `pip3` package manager.

### Installing Frida on a Jailbroken iOS Device

The repository 



## Setting up for Interception of HTTP(S) traffic

To be in a position to see the traffic from the mobile application. An interception proxy, such as Burp Suite, has to be used. Please see the section []() for setting up a remote SSH port forward on port "8080" and configure the device to 
