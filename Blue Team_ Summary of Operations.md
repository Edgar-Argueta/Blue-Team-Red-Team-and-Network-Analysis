Blue Team: Summary of Operations
**Table of Contents**
* Network Topology
* Description of Targets
* Monitoring the Targets
* Patterns of Traffic & Behavior
* Suggestions for Going Further

**Network Topology**
The following machines were identified on the network:
**Capstone**
Operating System: Ubuntu 18.04
Purpose: Vulnerable Web Server
IP Address:192.168.1.105

**Kali**
Operating System: Debian Kali 5.4.0
Purpose: Pen Tester
IP Address:192.168.1.90

**Elk Stack**
Operating System: Ubuntu 18.04
Purpose: Elasticsearch and Kibana
IP Address: 192.168.1.100

**Target 1**
Operating System: Debian GNU/Linux 8
Purpose: WordPress Host
IP Address: 192.168.1.110

**Description of Target**
The target of this attack was: Target 1 (192.168.1.110).
Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:
**Monitoring the Targets**
Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:
**Excessive HTTP Errors**
Excessive HTTP Errors is implemented as follows:
Metric: WHEN count() GROUPED OVER top 5 'http.response.status_code' 
Threshold: IS Above 400
Vulnerability Mitigated: Brute Force and User Enumeration 
Reliability: Strong; For this attack, 400+ and 500+ will alert when a possible attack is happening and also filter out normal activity. 

**HTTP Request Size Monitor**
HTTP Request Size Monitor is implemented as follows:
Metric: WHEN sum() of http.request.bytes OVER all documents
Threshold: IS ABOVE 3500
Vulnerability Mitigated: Code Injection and DDOS
Reliability: Moderate; While it will set of alarms for events that is being monitored, there is a possibility of false positives.

**CPU Usage Monitor**
CPU Usage Monitor is implemented as follows:
Metric: WHEN max() OF system.process.cpu.total.pct OVER all documents
Threshold: IS ABOVE 0.5
Vulnerability Mitigated: Malware
Reliability:  Very reliable; Also can help efficiency for looking at CPU USAGE.

**Suggestions for Going Further**
Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. 
The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:

**Weak Passwords**
Create a password policy to include a minimum of 10 characters. Within the 10 characters, it must include one capital letter, one number, and one special character. Another policy to be implemented is that passwords be changed ever 60 days. Finally, create an alert that notifies the SOC team when 3 wrong attempts have been done to the user account.
Why It Works: This works because it protects the user account from a variety of brute force attacks. If the account is breached, the SOC team can mitigate the damage by responding back quickly.

**Unsalted Hash Password**
One of the things to do is to upgrade MySQL from version 5.7 to version 8.0.29. SHA-2 encryption can be used to salt all passwords. MySQL version 8.0.29 has a plugin that is built in to easily use. 
Playbook Patch
  ---
  - name: MySQL Version update
    hosts: webservers
    become: true
    tasks:
    
    - name: stop MySQL
      service:
        name: mysql
        state: stopped

    - name: Download MySQL Repository:
      comand: curl -L -O https://dev.mysql.com/get/Downloads/MySQL-8.0/libmysqlclient21_8.0.29-1debian11.10_amd64.deb
    
    - name: Install MySQL Package
      command: dpkg -i libmysqlclient21_8.0.29-1debian11.10_amd64.deb
     
    - name: Update Package Information from MySQL APT Repository
      shell: apt-get update

    - name: Upgrade MySQL server 
      command: apt-get install mysql-server
   
    - name: Restart MySQL
      service: 
        name: mysql
        state: started
Why It Works: This will salt all passwords making it harder to decrypt. It will also reduce or eliminate the threat of user enumeration attack.

**Mismanaged User Privileges**
Create a policy that ensures that users get the proper privileges that are needed to do their tasks. Also create a password to run root features. 
Why It Works: Creating this type of policy reduces the chances of an attacker being able to traverse, access, and modify files from a user account to otherwise unauthorized sections.


