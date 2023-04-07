# Bypassing SSL Certificate Pinning Protections Multiple Ways

## What is SSL Certificate Pinning

SSL Certificate pinning is a machanism that protects against the interception of HTTPS (TLS/SSL) traffic on a mobile device. A certificate or "public hash" of an SSL certificate is embedded within the application binary. When the application initiates a connection to the target server, the certificate/hash is checked. If it does not pass verification - for example because of an attacker attempting to man-in-the-middle the traffic - the connection will be terminated and the application will not be able to connect to the web server.

To test the backend of an iOS application, which could be a web server running an API, SSL pinning needs to be bypassed, which can be carried out in a number of ways.

## Installing the Required Tools

The Frida frameworks need to be installed on both the iOS device, using Cydia, and Linux system using Python 3's `pip3` package manager. The tool `Objection` will need to be installed using Python 3's `pip` package manager too, which acts as a wrapper around Frida, to make interaction with the framework easier.

### Installing Frida on a Jailbroken iOS Device

Open Cydia and select "Sources", click "Add" and add the following repository:

`https://build.frida.re`

Click "Sources" and press  "Refresh". This will refresh all sources in Cydia. 

Search for "Frida" and click install. The latest version - at the time of writing - is `Frida v16.0.11`

### Installing Frida and Objection on the Linux host

#### Frida installation

Frida can be installed using the `pip3` Python 3 package manager. It is important that the version installed on Linux matches the version installed on the iOS device. Otherwise the setup may not work!

To install the correct version of Frida using "pip3" type in a Linux terminal:

`pip3 install frida==16.0.10`

By specifying `==16.0.10` you can select the exact version of Frida to install that matches the iOS device.

Now install "frida-tools

`pip3 install frida-tools`

#### Objection Installation

Now the "objection" tool can be installed, objection acts as a wrapper around Frida to make usability easier:

`pip3 install objection`


## Setting up for Interception of HTTP(S) traffic

To be in a position to see the traffic from the mobile application. An interception proxy, such as Burp Suite, has to be used. Please see the section []() for setting up a remote SSH port forward on port "8080" and configure the device to 
