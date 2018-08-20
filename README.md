# LinuxONE-CaaS
LinuxONE Crypto as a service with NodeJS and Node-Red

# Architecture
In this code pattern:
1. You will install Node.js and Node-RED
2. You will get install additional library for Encryption. 
3. You will create and encryption framework with Node-RED.
4. User to be able to benefit Crypto as a Service on LinuxONE.

# Step 1 — Installing Node.js and npm

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

# Step 2 — Installing Node-RED

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

Some "Welcome to Node-RED" messages will print to the terminal. On your computer, point a web browser to port 1880 of the server. In our example, that's http://node-red.example.com:1880. The main admin interface of Node-RED will load.

We've installed Node-RED successfully and tested it out, so next, we'll add additional crypto libraries to Node-RED.

# Step 3 — Installing node-red-contrib-crypto-js
Node-RED nodes using CryptoJS to encrypt and decrypt messages.
```
npm install node-red-contrib-crypto-js
```

