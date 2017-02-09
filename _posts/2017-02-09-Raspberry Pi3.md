---
layout:     post
title:      Starting with Raspberry Pi 3
date:       2017-02-09 23:30:27
summary:    This post describes how to configure and setup a Raspberry Pi 3 and proceed to implement a home automation system
categories: Raspberry Pi 3 Model B, Internet of Things, Python
---

In this post, I'm going to set up and configure a Raspberry Pi 3 Model B, working on the Raspbian OS, connect to the internet via ethernet and establish SSH (Secure Shell) connection.

Raspberry Pi is a mini-computer capable of small and medium-scale processing. The two best things about a Raspberry Pi are that it is compact and cheap. It comes equipped with Bluetooth, Wifi, 1 GB RAM and 4 USB 3.0 ports. To make it a full fledged PC, all that is needed to be done after the setup and installation is to connect it to  a monitor, a keyboard and a mouse. I've started a new project to convert the traditional appliances in a generic household to smarter remote controlled devices, capable of being accessed by the privileged user from anywhere in the world. Imagine to be able to control the old school heating rod or your ancient toaster- all that at a push of a button from anywhere. The idea is to design embedded plugs that can be mounted to any socket and device. These plugs are connected and controlled by the Raspberry Pi, and the user can send desired commands to the Raspberry Pi, which then propogates the command to the relevant plug. The project is centered around Rasberry Pi and hence, I will be describing the setup and installation of the credit-card sized device in this post.

I bought a Raspberry Pi 3 Model B from Amazon at 2890 INR. An additional keyboard, a mouse, an 8GB microSD card, an HDMI cable and an ethernet cable was also purchased for an additional 990 INR. I used my LED TV to serve the purpose of a monitor. This makes it a total investment of just 3880 INR! 

After collecting the required harware, I started with the software on my Windows 10 PC. First of all, you need to download the OS image file for the Raspberry Pi. I installed my Pi with Raspbian, the official Raspberry Pi foundation's operating system. I downloaded the Raspbian Jessie with pixel since I wanted to use my Pi as a generic full fleged PC at home. You can download the Lite version if you like, or any other OS for that matter. The Rasbian OS download link is <a href="https://www.raspberrypi.org/downloads/raspbian/"><i>here</i></a>. Unzip the downloaded file, and save it. Now download Win32DiskImager tool from <a href="https://sourceforge.net/projects/win32diskimager/files/latest/download"><i>here</i></a>. The purpose of this tool is to upload the download OS image up into the microSD card connected to the computer via microSD Adapter. If you already have a microSD card that contains some data, you can back up the data and use <a href="https://www.sdcard.org/downloads/formatter_4/"><i>SDFormatter</i></a> to format and clean your SD card. After Win32DiskImager is installed and opened, select the OS image and the corresponding drive you want it loaded in and click the <b>Write</b> button. This loads up your SD card with the operating system for Raspberry Pi. Now put this card into the microSD card slot available on the lower side of the Pi. Then, connect the Raspberry Pi to a monitor (a simple home LED did the job right) via HDMI cable, and connect the keyboard and mouse to the USB ports. Also, connect the Raspberry Pi to the home router via Ethernet cable. Finally, connect the power port and let the Pi boot up. This process should take up a few minutes. And congratulations, you have successfully set up your own PC with Raspberry Pi.

The default username and password for the Raspberry Pi is <b>pi</b> and <b>raspberry</b> respectively. To be able to remotely login to the Pi from the convenience of your laptop or PC, you'll need to do setup SSH connection. The Raspbian OS comes pre-loaded the OpenSSH server to be able to do SSH in remote machines. If you have installed any other OS, it may or may not come with the pre-requisites so open the terminal in the Raspberry Pi to update and load the required software packages:

	
	pi@raspberrypi$ sudo apt-get update && sudo apt-get upgrade
	
	pi@raspberrypi$ sudo apt-get install openssh-server openssh-client
	


After the above command is executed successfully, go back to your laptop/PC and download Advanced IP Scanner from <a href="http://filehippo.com/download_advanced_ip_scanner/?utm_source=FT&utm_medium=Redirect&utm_campaign=AIS"><i>here</i></a>. This tools provides a list of all connected devices to a network along with their corresponding dynamic IP address. After downloading and installing the Advanced IP Scanner, run and scan to find the IP address of the raspberry pi. <b>Make sure that both the PC and the Raspberry Pi are connected on the same network connection</b>. Fetch and note the IP address corresponding to the device name <b>raspberrypi</b>. If you're using a Mac or Linux, then you can directly use the terminal by typing <b>ssh -i pi@**Raspberry Pi IP**</b> and provide the password in the second line. If yo're using Windows, then you can download the SSH client PuTTY from <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html"><i>here</i></a> and install the application. Just enter the IP of the remote host and enter the required credentials. And that's it! You have successfully setup and remotely accessed your Raspberry Pi.

The Raspbian OS comes preloaded with Java, Scala and Python (both version 2.7 and version 3.4.2) along with some more. I'll be using Python to build my project on the side of the Raspberry Pi. If you used a different OS image and it doesn't have python installed, you can do it yourself by:

	
	pi@raspberrypi$ sudo apt-get install python3.4

	pi@raspberrypi$ sudo apt-get install python-pip
	

To get started with coding Raspberry Pi, we'll also need to download the corresponding Python package by:

	
	pi@raspberrypi$ pip install RPi.GPIO
	

So this wraps up this post. I am currently working this package to be able to send data across to a back-end system for processing based on certain input values at GPIO pins. After that, I'll try to make this interaction wireless. I will describe these in more detail in upcoming posts...