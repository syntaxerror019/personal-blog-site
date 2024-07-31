---
title: "Connecting to Oracle Cloud servers with VNC"
date: 2024-07-31T13:03:04-04:00
draft: false
excerpt: "SSH-ing is easy. But what about your GUI apps?!"
tags: ["Oracle Cloud", "VNC", "SSH", "OCI", "Server"]
featured: false
---
## What is Oracle Cloud?
**Oracle Cloud** is a cloud computing service offered by Oracle Corporation. It provides a range of cloud services, including:

- **Infrastructure as a Service (IaaS)**: This includes virtual machines, storage, and networking resources.
- **Platform as a Service (PaaS)**: Offers tools for developing, managing, and deploying applications, including databases, application development, and integration services.
- **Software as a Service (SaaS)**: Provides ready-to-use software applications hosted in the cloud, such as enterprise resource planning (ERP), human capital management (HCM), and customer relationship management (CRM) applications.

Oracle Cloud is known for its strong emphasis on database solutions, high-performance computing, and enterprise applications. It aims to help businesses reduce IT costs, scale their operations, and innovate faster by leveraging cloud technologies.

## What is a VNC?
**VNC (Virtual Network Computing)** is a graphical desktop-sharing system that allows remote control of a computer over a network. It operates by transmitting the keyboard and mouse events from one computer to another, and relaying the graphical screen updates back in the other direction. 

Key features of VNC include:

- **Remote Access**: Users can access and control a remote computer from anywhere.
- **Cross-Platform Compatibility**: VNC clients and servers are available for various operating systems, including Windows, macOS, and Linux.
- **Security**: VNC can be secured using encryption and authentication methods to protect the connection.

Common uses of VNC include remote technical support, accessing files and applications on a remote machine, and managing servers.

---------------------------------------

# Introduction
SSH-ing into OCI (Oracle Cloud Infrastructure) servers is a breeze. Simply download the private key file, and load it into your favorite SSH client software. Boom! You're in and off you go!

But what if your application needs... GUI

Personally having a difficult time running GUI applications on my Free-Tier server, I found that a VNC connection would be necessary. After looking online for tutorials or general help, I quickly realized there was no straight answer. People said things that contradicted one another, and none of their ideas worked. In the end, I figured it out. I write this in hopes of helping those few people like me struggling with similar problems.

## Supplies
- You will need a running OCI server instance (Linux in this case (e.g. Ubuntu))
- A working SSH connection to set up the VNC software and perform necessary installs.
- A VNC viewer for testing and eventually establishing connection when appropriate.
- This tutorial...😂

## Connecting to Your Server Via SSH
If you have not already established a connection with your server, now is the time to do so! If you already have, skip to the next step.  

Using your favorite SSH client software (I will be using PuTTY for Windows) enter the IP address and upload the proper authentication credentials (Private key file)  

If you are using PuTTY, you will need to use the built-in PuTTYgen software that comes with PuTTY during install. The .key file provided by OCI does not work by default in PuTTY. Using PuTTYgen, you can convert it.  

Open PuTTYgen and load the .key file provided by OCI during the server instance creation.  
![PuttyGen](https://content.instructables.com/FED/JPGD/LZ8FHYNG/FEDJPGDLZ8FHYNG.png?auto=webp&frame=1&fit=bounds&md=eca4267c9d7f67ae14ed20eb297be9d8)
![PuttyGenLoadingFile](https://content.instructables.com/FWC/4SK0/LZ8FHYNS/FWC4SK0LZ8FHYNS.png?auto=webp&frame=1&fit=bounds&md=57b4fb531e7bb8389e3cd3ec81da3593)

Follow the pop-up when it appears.  
![PUTTYpopup](https://content.instructables.com/F7N/ZSDD/LZ8FHYO0/F7NZSDDLZ8FHYO0.png?auto=webp&frame=1&fit=bounds&md=c6b0dcef32fbf4be6fb23f48d7fbcbfa)

Click Save private key button to save it to your computer. This will now work in PuTTY.

Now, open the main PuTTY software and load the private key. This can be done by navigating through the tree menu in PuTTY to:
**Connection > SSH >Auth > Credentials**
![PuttyLoadFile](https://content.instructables.com/FHO/ANOC/LZ8FHYNA/FHOANOCLZ8FHYNA.png?auto=webp&frame=1&fit=bounds&md=4e46743963271fb53c1e812cbb2bd57e)

Once the file is loaded, be sure the correct IP address is saved (in the main session tab)
![PuttyIPconfig](https://content.instructables.com/FJQ/YWRT/LZ8FHYNB/FJQYWRTLZ8FHYNB.png?auto=webp&frame=1&fit=bounds&md=848604648564ffd3c576815482b5d6f7)

Save the PuTTY config with a name if desired.  
Press Open to initialize the connection to the server...  
![PuttyOpen](https://content.instructables.com/FUE/RX31/LZ8FHYO1/FUERX31LZ8FHYO1.png?auto=webp&frame=1&fit=bounds&md=8284f39b1b95d5938ffb7505e65889c3)

If a message appears warning you about how the key is not in the cache, ignore it and click Accept connection.  

---

## Install the VNC Server
Once connected to your server via SSH, we can start running commands to install the VNC server.  

Run the following command to update and upgrade your system packages/ dependencies. (This step IS important!)  
```bash
sudo apt-get update && sudo apt-get upgrade
```  

Install the VNC server and lightweight display manager. We will use TightVNC here as it is lightweight and easy to use.
```bash
sudo apt-get install lxde-core lxterminal tightvncserver
```  

Set up the VNC server password by running: (Type "n" if it asks if you want a view-only password)
```bash
vncserver
```  

End that VNC instance, as we need to configure it a bit more:
```bash
vncserver -kill :1
```  

---

## Set Up VNC Startup Script
Copy the existing startup command as a backup just in case:
```bash
mv ~/.vnc/xstartup ~/.vnc/xstartup.bak
```

Create a new startup scrpt: (This is the one that will be actually used):
```bash
nano ~/.vnc/xstartup
```

Add the following lines to the file and save it (Save it by pressing CTRL+X, y, Enter) [Don't forget the shebang too!]
```bash
#!/bin/bash
xrdb $HOME/.Xresources
startlxde &
```

Make the script executable:
```bash
chmod +x ~/.vnc/xstartup
```

---

## Configure Oracle Ingress Rules (VCN)
In order for the VCN (not to be confused with VNC lol 😂) to allow external connections on port 5901 (default VNC port) you will need to configure it appropriately. *This can be tricky so follow this Step-By-Step to ensure proper configuration!*  

Log into the Oracle Cloud dashboard and navigate to the Virtual cloud networks (VCN) page.
![OracleVCN](https://content.instructables.com/FRI/WTIP/LZ8FI2KI/FRIWTIPLZ8FI2KI.png?auto=webp&frame=1&width=1024&fit=bounds&md=098c7b2adaf25ecc9794e60eb832e890)  

Click the main VCN link. For example [vcn-20240307-2120](https://sntx.dev/)  
Click the main subnet link. For example [subnet-20240307-2120](https://sntx.dev/)  
Click the Default Security List link. For example [Default Security List for vcn-20240307-2120](https://sntx.dev/)  

And finally you are at the Ingress Rules page of your VCN! Click the "Add Ingress Rules" button  
![IngressAdd](https://content.instructables.com/F4N/KPE9/LZ8FI2L3/F4NKPE9LZ8FI2L3.png?auto=webp&frame=1&width=1024&fit=bounds&md=6953055a48161c44115add5ccef88d5b)  

And follow the configuration specified below. (This is very important!)
- **Stateless:** Checked
- **Source Type:** CIDR
- **Source CIDR:** 0.0.0.0/0
- **IP Protocol:** TCP
- **Source port range:** (leave-blank)
- **Destination Port Range:** 5901
- **Description:** VNC_SETUP

Now, if done correctly, you should have an ingress rule that looks something like the following:
![Ingress](https://content.instructables.com/FHJ/QVJI/LZ8FI2LP/FHJQVJILZ8FI2LP.png?auto=webp&frame=1&width=1024&fit=bounds&md=5e011100d752492e9418eb6d798da58c)  

Here are the [official Oracle Cloud docs](https://docs.oracle.com/en-us/iaas/developer-tutorials/tutorials/apache-on-ubuntu/01oci-ubuntu-apache-summary.htm) on a related topic.

---

## Configure Ubuntu Firewall
Once you have finished setting up the Ingress Rules, jump back to the SSH window to execute a couple more commands on you server to get it working with the VNC setup.  

Run the following commands in your terminal (via SSH) to let the Ubuntu iptables know that port 5901 should allow traffic:
```bash
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 5901 -j ACCEPT
sudo netfilter-persistent save
```  

Here are the [official Oracle Cloud docs](https://docs.oracle.com/en-us/iaas/developer-tutorials/tutorials/apache-on-ubuntu/01oci-ubuntu-apache-summary.htm) on a related topic.

---

## Start VNC Server and Connect!
Great! You've done it! 🥳👏 All you need to do now is start up the server and connect.  

In the SSH terminal, type:
```bash
vncserver
```
or for additional configuration use this variation: (This example is suitable for limited-resource servers)
```bash
vncserver :1 -geometry 1024x768 -depth 16
```  

Now, open up a VNC viewer/ client on your local machine (I used RealVNC, a popular choice) and enter the Server's IP address in the top bar.  
![VNCIP](https://content.instructables.com/F7O/09FR/LZ8FI2LR/F7O09FRLZ8FI2LR.png?auto=webp&frame=1&fit=bounds&md=1b6d4f03d30cd21b665091931b1dc205)

Hit enter, and the connection should be starting. If you see warnings about encryption, press Continue.  
![VNCWARN](https://content.instructables.com/FFJ/YT2D/LZ8FI2R5/FFJYT2DLZ8FI2R5.png?auto=webp&frame=1&fit=bounds&md=8a5779ba98c7db74cbe6e79274628c4a)

When prompted to enter your password, it is the same one you entered back in step 2. It is NOT the same one as your server password (unless you made them the same...)  
![VNCPASS](https://content.instructables.com/FFG/L78X/LZ8FI2RX/FFGL78XLZ8FI2RX.png?auto=webp&frame=1&fit=bounds&md=5c2a631f87fd6192fffe2629ce5500f4)

You should now see your Server's Desktop! Congratulations! You've done it! 🥳😁🙌👌👏  

![VNCDESKTOP](https://content.instructables.com/FMI/TLFQ/LZ8FI2RY/FMITLFQLZ8FI2RY.png?auto=webp&frame=1&width=1024&fit=bounds&md=083c90968662776d295b9290acc85181)  

If you encounter any issues, please leave a comment below! Thanks!