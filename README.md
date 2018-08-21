# LinuxONE-CaaS

When you will complete this hands-on exploration of Crypto as a Service on LinuxONE, you will understand how to:
* Preparing your Linux Environment to use hardware crypto
* Enabling OpenSSL to use the Hardware
* Installing NodeJS and Node-Red
* Creating local encryption capabilities
* Installing Mosquitto
* Creating remote encryption capabilities

# Architecture
This journey requires an existing Linux on IBM Z or LinuxONE environment of your choice as a starting point. From there, and after some optimization, you will be able to set-up a new Crypto as a Service to offer to processor architecture not as performing as LinuxONE or IBM Z.
![alt-text](https://github.com/guikarai/LinuxONE-CaaS/blob/master/images/node-red-linuxone-architecture.png)

In this code pattern:
1. User installs Node.js, Node-RED, Mosquitto and additional library for NodeJS encryption. 
2. User creates and encryption framework with Node-RED.
3. User benefits Crypto as a Service on LinuxONE.

# Included components
* [LinuxONE Crypto](https://www.ibm.com/it-infrastructure/linuxone/capabilities/secure-cloud)
* [OpenSSL](https://www.openssl.org/)
* [NodeJS](https://nodejs.org/en/)
* [Node-Red](https://nodered.org/)
* [Mosquitto](https://mosquitto.org/)
* [node-red-contrib-crypto-js](https://flows.nodered.org/node/node-red-contrib-crypto-js)

# Featured technologies
* [IBM LinuxONE](https://www.ibm.com/it-infrastructure/linuxone)

# Steps

## Step 1 — Installing Node.js and npm

Node-RED is a switchboard a visual tool that helps you connect your favorite apps, websites, and hardware together to do new and useful things. Node-RED has a much more powerful and flexible interface, and a large open source community creating nodes to interact with a wide variety of apps and services.

Ubuntu makes easy the install of Node.js because it's included in the default repository. Please issue the following command:
```
sudo apt-get install nodejs-legacy
```

Verify that the installation was successful by checking the version. Please issue the following command:
```
node -v
```

Node Package Manager (npm) helps you install and manage Node.js software packages, and we'll use it to install Node-RED. Install npm using apt-get.
```
sudo apt-get install npm
```

To verify the install was successful, ask npm to print its version information:
```
npm -v
```

If it prints a version number without error, we can continue on to our next step, where we'll use npm to install Node-RED itself.

## Step 2 — Installing Node-RED

Use npm to install node-red and a helper utility called node-red-admin.
```
sudo npm install -g --unsafe-perm node-red node-red-admin
```

npm normally installs its packages into your current directory. Here, we use the -g flag to install packages 'globally' so they're placed in standard system locations such as /usr/local/bin. The --unsafe-perm flag helps us avoid some errors that can pop up when npm tries to compile native modules (modules written in a compiled language such as C or C++ vs. JavaScript).

After a bit of downloading and file shuffling, you'll be returned to the normal command line prompt. Let's test our install.

First, we'll need to open up a port on our firewall. Node-RED defaults to using port 1880, so let's allow that.

```
sudo ufw allow 1880
```

And now launch Node-RED itself. No sudo is necessary, as port 1880 is high enough to not require root privileges.
```
node-red
```

Some "Welcome to Node-RED" messages will print to the terminal. On your computer, point a web browser to port 1880 of the server. In our example, that's http://yourIP-address:1880. The main admin interface of Node-RED will load.

We've installed Node-RED successfully and tested it out, so next, we'll add additional crypto libraries to Node-RED.

## Step 3 — Installing node-red-contrib-crypto-js

Connect to your node-red service on port 1880.
eg. http://yourIP-address:1880

The landing page of an up and running Node-RED looks like the following:

![alt-text](https://github.com/guikarai/LinuxONE-CaaS/blob/master/images/node-red-landing-page.png)

**Action:** Please, on the top-right-bar, click on **Menu**, and then **Manage palette**.

![alt-text](https://github.com/guikarai/LinuxONE-CaaS/blob/master/images/node-red-right-menu-bar.png)

**Action:** Click on **Install** tab, enter as shwo below in the search bar: node-red-instal-crypto-js. Install the two listed packages.
![alt-text](https://github.com/guikarai/LinuxONE-CaaS/blob/master/images/node-red-instal-crypto-js.png)

## Step 4 - Creating your local first encryption flow

## Step 5 - Installing and configuring Mosquitto

### What is Mosquitto?
Eclipse Mosquitto is an open source (EPL/EDL licensed) message broker that implements the MQTT protocol versions 3.1 and 3.1.1. Mosquitto is lightweight and is suitable for use on all devices from low power single board computers to full servers.

The MQTT protocol provides a lightweight method of carrying out messaging using a publish/subscribe model. This makes it suitable for Internet of Things messaging such as with low power sensors or mobile devices such as phones, embedded computers or microcontrollers.

The Mosquitto project also provides a C library for implementing MQTT clients, and the very popular mosquitto_pub and mosquitto_sub command line MQTT clients.

Mosquitto is part of the Eclipse Foundation and is an iot.eclipse.org project.

### Installing Mosquitto
```
sudo apt-get install mosquitto mosquitto-clients
```

Note that, once downloaded the Mosquitto broker starts straight away so we will stop it to make some changes first. 
**Action:** Please issue the following command:

```
sudo /etc/init.d/mosquitto stop
```
### Configuring Mosquitto

Now that the MQTT broker is installed we will add some basic security.
**Action:** Create a config file with the following command:
```
cd /etc/mosquitto/conf.d/
sudo vi mosquitto.conf
```

Let's stop anonymous clients connecting to our broker by adding a few lines to your config file. To control client access to the broker we also need to define valid client names and passwords. 
**Action:** Please, add the lines:
```
allow_anonymous false
password_file /etc/mosquitto/conf.d/passwd
require_certificate false
```

**Action:** Save and exit your editor (vi in this case).

**Action:** From the current /conf.d directory, create an empty password file:
```
sudo touch passwd
```

We will to use the mosquitto_passwd tool to create a password hash for user root:
```
sudo mosquitto_passwd -c /etc/mosquitto/conf.d/passwd root
```

You will be asked to enter your password twice. Enter the password you wish to use for the user you defined.
### Testing Mosquitto

Now that Mosquitto is installed we can perform a local test to see if it is working.

**Action:** Open three terminal windows. In one, make sure the Mosquitto broker is running. Please issue the following command:
```
mosquitto
```

**Action:** In the next terminal, run the command line subscriber. Please issue the following command:
```
mosquitto_sub -v -t 'topic/test'
```

You should see the first terminal window echo that a new client is connected.

**Action:** In the next terminal, please run the command line publisher:
```
mosquitto_pub -t 'topic/test' -m 'hello World'
```
You should see another message in the first terminal window saying another client is connected. You should also see this message in the subscriber terminal:
```
topic/test hello World
```

We have shown that Mosquitto is configured correctly and we can both publish and subscribe to a topic.

## Step 6 - Creation your first remote encryption flow
