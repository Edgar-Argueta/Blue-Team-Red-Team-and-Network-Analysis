**Network Analysis**

**Time Thieves**

At least two users on the network have been wasting time on YouTube. Usually, IT wouldn't pay much mind to this behavior, but it seems these people have created their own web server on the corporate network. So far, Security knows the following about these time thieves:
They have set up an Active Directory network.
They are constantly watching videos on YouTube.
Their IP addresses are somewhere in the range 10.6.12.0/24.
You must inspect your traffic capture to answer the following questions:

**What is the domain name of the users' custom site?**

Frank-n-ted-dc.frank-n-ted.com

![alt text](https://github.com/Edgar-Argueta/Blue-Team-Red-Team-and-Network-Analysis/blob/973d816b49fb5ee2e79676c2e6298bf4482b5b6a/Network%20Analysis%20Screenshots/Wireshark_DNS_Frank.png)

**What is the IP address of the Domain Controller (DC) of the AD network?**

The IP address of the DC of the AD network 10.6.12.12

![alt text](https://github.com/Edgar-Argueta/Blue-Team-Red-Team-and-Network-Analysis/blob/973d816b49fb5ee2e79676c2e6298bf4482b5b6a/Network%20Analysis%20Screenshots/Domain_Controller.png) 

**What is the name of the malware downloaded to the 10.6.12.203 machine?**

The malware file is june11.dll

Uploading the file to VirusTotal.com the type of malware that the file was classfieid was as a Trojan.

![alt text](https://github.com/Edgar-Argueta/Blue-Team-Red-Team-and-Network-Analysis/blob/973d816b49fb5ee2e79676c2e6298bf4482b5b6a/Network%20Analysis%20Screenshots/Malware_June11.dll.png)
 
 
**Vulnerable Windows Machines**

The Security team received reports of an infected Windows host on the network. They know the following:
Machines in the network live in the range 172.16.4.0/24.
The domain mind-hammer.net is associated with the infected computer.
The DC for this network lives at 172.16.4.4 and is named Mind-Hammer-DC.
The network has standard gateway and broadcast addresses.
Inspect your traffic to answer the following questions:

**Find the Host name, IP address, and MAC address about the infected Windows machine**

* Host name: ROTTERDAM-PC
* IP address:172.16.4.205
* MAC address: 00:59:07:b0:63:a4
* Filter used was ip.addr=172.16.4.4 && eth.src

![alt text](https://github.com/Edgar-Argueta/Blue-Team-Red-Team-and-Network-Analysis/blob/973d816b49fb5ee2e79676c2e6298bf4482b5b6a/Network%20Analysis%20Screenshots/Source_Host_MAC.png)

**What is the username of the Windows user whose computer is infected?**
 The username is matthijs.devries
* Used filter ip.addr == 172.16.4.205 && kerberos.CNameString

![alt text](https://github.com/Edgar-Argueta/Blue-Team-Red-Team-and-Network-Analysis/blob/973d816b49fb5ee2e79676c2e6298bf4482b5b6a/Network%20Analysis%20Screenshots/Username_Wireshark.png)

**What are the IP addresses used in the actual infection traffic?**

The IP addresses used in the actual infection traffic are 172.16.4.205, 185.243.115.84, and 166.62.111.64. Using the Statistics tab and then going to Conversations, and sorting by highest amount of packets revealed the answer.

![alt text](https://github.com/Edgar-Argueta/Blue-Team-Red-Team-and-Network-Analysis/blob/973d816b49fb5ee2e79676c2e6298bf4482b5b6a/Network%20Analysis%20Screenshots/172.16.4.205%20traffic.png) 
 
**Illegal Downloads**

IT was informed that some users are torrenting on the network. The Security team does not forbid the use of torrents for legitimate purposes, such as downloading operating systems. However, they have a strict policy against copyright infringement.
IT shared the following about the torrent activity:
The machines using torrents live in the range 10.0.0.0/24 and are clients of an AD domain.
The DC of this domain lives at 10.0.0.2 and is named DogOfTheYear-DC.
The DC is associated with the domain dogoftheyear.net.
Your task is to isolate torrent traffic and answer the following questions:

**Find the MAC address, Windows username, and OS version about the machine with IP address 10.0.0.201**
MAC address 00:16:17:18:66:c8
Windows username: elmer.blanco
OS version: Windows NT 10.0; Win64; x64

![alt text](https://github.com/Edgar-Argueta/Blue-Team-Red-Team-and-Network-Analysis/blob/61a25dc27d342f02d3c1e5abeea5d5d7a1a9f8c2/Network%20Analysis%20Screenshots/Elmer.blanco.png)
![alt text](https://github.com/Edgar-Argueta/Blue-Team-Red-Team-and-Network-Analysis/blob/61a25dc27d342f02d3c1e5abeea5d5d7a1a9f8c2/Network%20Analysis%20Screenshots/User_Agent_OS.png)

**Which torrent file did the user download?**

The user downloaded Betty Boop Rhythm on the Reservation torrent file.
The filter used to find the torrent file was http.host == “publicdomaintorrents.info” && ip.addr == 10.0.0.201

![alt text](https://github.com/Edgar-Argueta/Blue-Team-Red-Team-and-Network-Analysis/blob/61a25dc27d342f02d3c1e5abeea5d5d7a1a9f8c2/Network%20Analysis%20Screenshots/Torrent_Download.png)

