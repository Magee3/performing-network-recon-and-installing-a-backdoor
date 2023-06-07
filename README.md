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






