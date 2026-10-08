MZ-UCA CLOUD ENGINEERING
# DAY 05 --- STUDENT HANDS-ON LABS Linux + Nginx Practical Engineering Lab

## LAB 01 Linux Environment Discovery
Objective: Before changing a system, learn how to identify the environment.
-uname -a
 
-cat /etc/os-release
 
-hostnamectl
 
-hostname
 
-ip addr & hostname -I
 
-df -h
 
-free -h
 
Record
Write down:
• Operating system: Ubuntu 26.04.1 LTS
• Hostname: nhlanhla01
• IP address:172.25.95.38
• Available disk space: 955G
• Available memory: 3.1 G
Engineering question: Why should an engineer inspect a system before making changes?
An engineer must inspect a system before performing changes to prevent disastrous downtime, avoid unpredicted cloud costs, and eradicate security vulnerabilities. It is essential to know the state (directory) you are working on as an engineer so that updates can be performed precisely and accurately, and it will be safer because no confidential or private information will be exposed to the public. Reviewing the current state of a system provides critical context that safeguards the infrastructure and the business.

## LAB 02 SSH Access
Objective: Practise securely accessing a Linux system remotely.
-ssh username@SERVER-IP
 
-sudo apt install openssh-server
 
  
-sudo service ssh status
 
-sudo service ssh start
 
-sudo service ssh status
 
-sudo service ssh start
 
-sudo service ssh status
 
-sudo ufw status
 
-sudo ufw allow 22/tcp
 




-sudo ufw status | grep :22
 


-sudo systemctl status ssh
 
-sudo systemctl enable --now ssh
 
-sudo cat /etc/ssh/sshd_config | grep port 
- ssh -v username@SERVER-IP
 
 
 














-ssh-keygen -f ‘/home/nhlanhla_01/.ssh/known_hosts’ -R ‘172.25.95.38’ 
-ssh -v nhlanhla_01@172.25.95.38
   
   






-ssh username@SERVER-IP
 
 
Engineering question: Why is it dangerous to make changes without confirming which server you are connected to?
Making changes without confirming your server can cause accidental downtime, data loss, or security breaches on the wrong system. Running a command to intentionally to test a server may disrupt a live production system which could result in site crashes or service outages. Overwriting existing files or databases on the wrong directories (not intended) can result in erasing vital data user or business records. 

## LAB 03 Install Nginx
Objective: Deploy your first web server.
-sudo apt update
 
-sudo apt install nginx -y
 
-sudo systemctl status nginx
 
 
 
 
-sudo systemctl start nginx
 
-sudo systemctl enable nginx
 
-curl localhost
 
## LAB 04 Understand the Nginx Service
Objective: Learn how Linux manages services.
-sudo systemctl status nginx
 
-sudo systemctl status nginx
     
-sudo systemctl start nginx
 
-sudo systemctl status nginx
 
What changed between active (running) and inactive (dead)?
The active (running) state means the Nginx service is currently up, serving web requests, and maintaining active master and worker processes in memory, whereas inactive (dead) means the service is completely stopped and not running.
Engineering principle: A service can be installed on a machine without currently
running.


## LAB 05 Build Your Own Web Page
Objective: Replace the default Nginx webpage with your own engineering page.
-sudo nano /var/www/html/index.html
 
 
 


-curl localhost
 
 
## LAB 06 Understand Ports
Objective: Understand how network services receive traffic.
-sudo ss -tulpn
 
 
-sudo ss -tulpn | grep -E ‘:22|:80’
 
Identify:
1. Which process is listening?
Nginx service is listsening for HTTP traffic and sshd nis listening for SSH traffic.
2. Which port is it using?
Nginx uses SSH uses Port 22 and Port 80 for HTTP.
3. Which protocol is being used?
TCP (transmission Control Protocol) is being used for both services.

Engineering question: Can an application be running but still unreachable over the
network? Why?
Yes, an application can be fully running yet completely unreachable over the network. 
This can be caused by:
-firewalls (like iptables, ufw or windows defender) blocking incoming traffic on the specific port the application uses.
-when a client attempts to connect to a different port than the one the application is actively listening on.
-network routing issues such asphtysical network issues, misconfigured VLANs or bad routing tables can prevent packets from reaching the host machine.
-NAT/Port forwarding failures,for example, if the app is behind a router or a proxy, missing or incorrect port-forwarding rules will drop external traffic before it reaches the app.

## LAB 07 Configure the Firewall
Objective: Practise basic network access control.
-sudo ufw status 
 
-sudo ufw allow ssh
 
-sudo ufw allow 80/tcp
 

-sudo ufw enable
 
-sudo ufw status
 
Engineering principle: Expose only what the workload requires.
LAB 08 Test Connectivity
Objective: Verify that your web server actually responds.
-curl localhost
 
-ip addr
 
-curl http://SERVER-IP
 
Record
Was the webpage accessible? YES / NO
Yes, the webpage was accessible.
If NO — What did you investigate?
The following can be checked:
-Check connectivity: ping 8.8.8.8 for internet connection. Run ip addr to verify network interfaces for proper configuration.
-DNS name resolution: sudo /etc/resolv.conf
-HTTP status and server response: curl localhost
-Firewalls and Local blockades: sudo ufw status 

## LAB 09 Access Logs
Objective: Observe incoming HTTP requests.
-sudo tail -f  /var/log/nginx/access.log
 
-curl localhost
 
Engineering principle: Every successful request can leave operational evidence.
LAB 10 Error Logs
Objective: Learn where application errors can be investigated.
-sudo tail -f /var/log/nginx/error.log
 
-sudo journalctl -u nginx
 
Questions
What is the difference between access.log and error.log? What type of information might
you expect in each?
An access.log records every incoming request to a web server, while an error.log captures server-side issues, failures, and configuration problems. 
•	An access log tracks traffic and visitor activity on a website or server. Each line represents a single HTTP request. 
-Client IP Address: The network address of the user or device making the request.
-Timestamp: The exact date and time when the server received the request.
-HTTP Method: The action requested, such as GET or POST.
-Requested URL/Path: The specific resource or page being accessed.
An error log tracks operational failures and diagnostic data rather than routine traffic. Each entry usually includes a severity level (such as warn, error, crit, or debug). In errog.log you may find:
-Server Failures: Critical issues like failed database connections or backend upstream timeouts.
-Configuration Issues: Syntax mistakes or invalid directives in the web server configuration files.
-Missing Files: Errors when a requested file or script cannot be located on the disk.
-Permission Problems: Denied access errors when the server lacks rights to read a file or directory

## LAB 11 Break the Web Server
Objective: Practise troubleshooting a controlled failure.

Step 1 --- Stop Nginx
-sudo systemctl stop nginx
 
Step 2 --- Test
curl localhost
 
Step 3 --- Investigate
Do not immediately restart it. Check:
sudo systemctl status nginx
 
sudo journalctl -u nginx
 
Step 4 --- Diagnose
Complete this statement:
The webpage is unavailable because nginx web server was inaccessible.  
Step 5 --- Recover
sudo systemctl start nginx
 
Step 6 --- Test again
-curl localhost
 
Expected result: Your webpage should work again.

## LAB 12 Troubleshooting Challenge

1. Service → 2. Port → 3. Network→ 4. Firewall → 5. Logs → 6. Configuration
-sudo systemctl status nginx
 
-sudo ss -tulpn
 
-ip addr
 
-sudo ufw status
 
-sudo journalctl -u nginx
 










Your task --- Write down:
Problem: The website is down. The web page is inaccessible. 
 
Evidence: The web server was inactive.
 
Root cause: Identify the cause of inaccessible web server. 
 
 
 

Resolution: The client is trying to access the web page from a port where the web server is not listening.
 
 
Verification: Check the status of the web server, verify the ports the web server is listening on.
 
 
 
## LAB 13 Engineering Evidence
 
 
 
 
 
 
  
 
 

