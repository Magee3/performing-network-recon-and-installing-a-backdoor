### Performing Network Recon and Installing A Backdoor

![Performing Network Recon and Installing A Backdoor](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/519c3498-6daf-44bb-84a3-aaf21456837e)

You will begin by investigating the network as if you were unfamiliar with it. You will use various tools to acquire and document the layout. You will then specifically investigate a web server. Finally, you'll participate in a test security features as related to malware that installs a remote backdoor. You will close the backdoor at the end of the exercise.


### Discovering The Network

 Reconnaissance, often referred to as "recon," is a critical phase in cybersecurity that involves gathering information about a target system or network. It plays a vital role in understanding the vulnerabilities, weaknesses, and potential attack vectors that adversaries might exploit. It is also the first step in the MITRE ATT&CK framework.
 
 Our kali machine is on the network we are performing recon on. We are gathering IP's of all the devices including our own. There are multiple ways to gather this information just like a math problem. Personally my goal is to gather all the information without having to ask or move from my desk. So lets see what we can do.
 
 Run:
 
 ifconfig
 
 ![cropped security 1-2](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/b112a595-164c-49ec-bac7-44e5e85c55e5)
 
 Our machines IP is 10.1.0.192
 
  We will now grab the IP's from all the devices on the Network. Run:
  
  nmap 10.1.0.0/24
  
![cropped security 1-3](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/f897505b-5562-4996-9616-5212c153c16b)

![cropped security 1-4](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/ead0a2bf-5833-4676-bf05-a5d58f55f128)

We are given Domain Names of devices, a list of ports, IP's, and MAC addresses.

For devices that are still questionable to us we can add in the aggressive switch "-A" to give us more details about the node.

![cropped security 1-6](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/57886eb3-45b9-4213-9689-775ceb995a9a)

We are given the same information with a little bit of extra such as Kernel info, OS version, and more.


### Gathering Info On The Web Server

We suspect that our MS1 maybe the webserver due to the types of ports that are open. We will further investigate.

Run:

nslookup 10.1.0.2

We are looking for the FQDN

![cropped security 1-8](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/498b62c1-8d25-4cb2-a1f7-7d3a9254e862)

FQDN: MS1.corp.515support.com

We will attempt to connect to the HTTP server by using cURL Run:

curl -s -I 10.1.0.2

![cropped security 1-9](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/40422879-2f46-45a8-9ed0-bed38e6c671b)

We are shown that MS1 is a webserver using Microsoft IIS 10

### Configure The Web Server For Authentication

In our MS1 Workstation we go into our IIS Manager and ensure Basic Authentication is on and everything else is disabled.

![cropped security 1-10](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/31ab1c09-4f56-467a-a63c-1528a12e5949)

Now we will capture packets using tcpdump on the webserver. tcpdump can only capture packets, we will use Wireshark to read and analyze the packets.

Run:

Go into a browser of your choice and open the webserver by typing 10.1.0.2 into the url.

![cropped security 1-13](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/dd44d5f2-17d8-4da6-8593-2095339822a7)

For testing purposes enter the wrong credentials.

![cropped security 1-14](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/000a003a-5dbf-4b1f-a6d5-86ee5fca7d7d)

![cropped security 1-15](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/cfcbb53f-7079-41ef-a520-fbfe38798635)

Press <strong>CTRL+C</strong> to stop the tcpdump

We will now open up Wireshark. Open the file that was created with tcpdump. (auth.txt)

![cropped security 1-16](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/06ad7e0a-98ae-4c6a-84d2-14a0443f41ab)

Select a packet that contains "GET / HTTP" and expand the "Hypertext Transfer Protocol"

![cropped security 1-17](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/01280357-b197-4524-95cf-dbd4b5a5355b)

![cropped security 1-18](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/ddebbca2-3b62-4f49-ae71-be2d0623e07f)

Expand the Authorization tab.

![cropped security 1-19](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/a0b265a8-8db0-43a0-bb3d-84412faaaf29)

Since HHTP is unsecured we were able to capture and analyze the packets using Wireshark and tcpdump. We can see the credentials that were attempted in broad daylight. 515support\pentester as the username and dr0w$$ap as the password.

### Install Malware

First we will move into our MS1 web server environment and disable the firewall using windows Powershell. Run:

Set-MpPreference -DisableRealTimeMonitoring $True

![cropped security 1-38](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/7b64aa16-1a88-4a2c-8b88-74574df66e75)

We now load up our malware on dvd and install it. There will be no yes or no or visual indication that the malware has been succesfully installed.

Open up task manager to find any suspicious programs running.

![cropped security 1-39](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/6b506f0f-7d26-4344-a2cf-cd1f55c5a862)

![cropped security 1-40](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/8733def6-c1c2-4d1b-871a-962e51a7d5ec)

ncat (32 bit) is our backdoor that is running. Lets see if it is operating how it should be. Switch back over to the kali workstation.

![cropped security 1-41](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/ecb210de-6b8a-4754-b9ac-ff22a8961f26)

Our malware is operating on port 4450/tcp and is currently open. We can move into loging into our backdoor.

  On another workstation we open putty and connect to the backdoor. Use IP 10.1.0.2, on Port 4450 using a RAW connection.
  
  ![cropped security 1-42](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/6755bda7-0b34-4b6e-a1c6-89e9463b98d7)
  
  ![cropped security 1-43](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/100ef078-2de9-47de-98fd-57c423ed0bbc)
  
  We have succesfully accessed our backdoor. It is now time to add a user account and assign it to a group.
  
  Run:
  
  net user /add mall Pa$$w0rd
  
  net localgroup administrators mal /add
  
   This will create a new user on the net with administrator priveleges. 
   
   ![cropped security 1-45](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/257f0c2a-3556-4a2c-b2b4-ef86de069113)
   
   ### Removing The Malware
   
   We will now move into removing the malware. We''ll first start by opening up "Task Manager" and killing the process.
   
   ![cropped security 1-46](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/38f4e16f-0c24-42df-bd85-04fe39d9377b)

   Next delete the "ini" file. It has all the contains the configurations for the malware.
   
   ![cropped security 1-47](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/df37a238-6238-4add-816f-a37164560b18)
   
   Open up the "Windows Firewall with Advanced Security" and look for any suspicious Inbound Rules. The Service Port looks suspicious to us. We open the rules and move into the "Protocols and Ports" to see if it is dealing with the same port the malware is.
   
   ![cropped security 1-49](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/9aae186b-8317-46ce-ac69-a74160b89509)

   We verify that this is the malware rule. The last step is to disable the rule.
   
   ![cropped security 1-48](https://github.com/Magee3/performing-network-recon-and-installing-a-backdoor/assets/134301259/53864f5c-bc16-475c-9a57-003140fed998)
   
   In summary, this exercise involved investigating an unfamiliar network using various tools to acquire and document its layout. We then focused on investigating a web server, followed by testing its security features against malware that installs a remote backdoor. Finally, we successfully closed the backdoor, effectively eliminating the potential risk of unauthorized access. Through this exercise, we enhanced the network's security and emphasized the importance of proactive measures to mitigate vulnerabilities.





  
  












