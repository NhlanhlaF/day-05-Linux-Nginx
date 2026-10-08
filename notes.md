# Day 05 - Linux + Nginx Lab

## Environment
Operating System: Ubuntu 26.04.1 LTS
Hostname:nhlanhla01
IP Address: 172.25.95.38

## Nginx
Installation: 
sudo apt update
sudo apt install nginx
sudo systemctl start nginx
 
Service Status: 
sudo systemctl status nginx

Web Root: sudo nano /var/www/html/index.html
Open and Save the following:

<!DOCTYPE html>
<html>
<head>
<title>MZ-UCA Cloud Engineering</title>
</head>
<body>
<h1>MZ-UCA Cloud Engineering Lab</h1>
<h2>Linux + Nginx</h2>
<p>Engineer: YOUR NAME</p>
<p>Environment: Ubuntu Linux</p>
<p>Day 05 Practical Engineering Laboratory</p>
</body>
</html>

## Networking
SSH Port:22
HTTP Port:80

## Firewall

Rules configured:
sudo ufw status
sudo ufw allow ssh
sudo ufw allow 80/tcp
sudo ufw enable
sudo ufw status

## Testing

Tests performed: 
curl localhost
ip addr
curl http://SERVER-IP
sudo systemctl status nginx
sudo journalctl -u nginx
sudo systemctl start nginx

## Troubleshooting

Failure: 
nginx.service
Error: Connection Refused

Evidence:
Test the Web server:
sudo systemctl status nginx
Result: Inactive (dead)

Stop conflicting services:
sudo systemctl stop nginx

Reload/Restart Nginx service:
sudo systemctl restart nginx

Identify existing errors:
sudo tail -f /vat/log/nginx/error.log

Root Cause:
Web server was listening in the wrong port. It was inaccessible.

Resolution:
sudo systemctl start nginx
sudo systemctl status nginx
## Lessons Learned
1. Always run sudo systemctl status nginx  before restarting the server 
2. Verify active network listeners by running the following command sudo ss -tulpn to check whether ports have binding conflict.
3. Configure the firewall to check if incoming traffic or outgoing traffic is blocked by running following commands to check status and activate it in case if it not activated: 
sudo ufw status
sudo ufw allow ssh
sudo ufw allow 80/tcp
sudo ufw enable 
