# BRG-27-labs
lab activities 
git clone https://github.com/pauyaahmanimaran-bumble/BRG-27-labs.git

__Student__: Pauyaah Manimaran <br>
__Kaplan ID__: CT0380229 <br>
__Module__: BRG-27 Introduction to Server Environments and Architectures <br>



## Session 1 – Linux Setup & Basic Commands

### Steps
- Launched Ubuntu EC2 instance on AWS.
- Connected via SSH:
  ```bash
  ssh -i mykey.pem ubuntu@<public-ip>

- Updated system:
`sudo apt update && sudo apt upgrade -y`

- Practised basic commands:

`pwd`
`ls -la`
`cd /var`
`mkdir testdir`
`touch file.txt`
`rm file.txt`


### Reflection

I learned how to navigate the Linux filesystem confidently. At first, I kept forgetting to use sudo, but after a few mistakes, I understood the importance of permissions.    This helped me to practice my skills and navigate through the filesystem by making mistakes, which allowed me to do the later sessions easily. I also realise that a new desktop was opened. It was totally different from my normal computer system. Even when taking screenshots, the screenshots get saved in that filesystem, which was also confusing. 


## Session 2 – Web Server Setup

### Steps
- Installed Nginx:

`sudo apt install nginx -y`

-  Verified service:

`systemctl status nginx`

- Created a simple HTML page:

`echo "<h1>Hello from Nginx</h1>" | sudo tee /var/www/html/index.html`


- Tested in browser: http://<public-ip>

### Reflection
As this was my first time seeing my own personal page load from the cloud which was fun and exciting. In the start, I had a hard time with firewall rules initially, but after opening port 80 in the AWS security group, everything worked. I thought that this session was a continuation of session 1. I kept using the command panel in that system. This session made me feel like I was really hosting something on the internet, which was really cool. 



# 3a DNS Setup 
Root domain bridging.linkpc.net → EC2 public IP 3.27.10.254.

Subdomain www.bridging.linkpc.net included via SAN.

Verified with nslookup and external SSL checkers.

3b SSL Certificate & Nginx

Installed Certbot:

`sudo apt install certbot python3-certbot-nginx -y`

Issued certificate:

`sudo certbot --nginx -d bridging.linkpc.net -d www.bridging.linkpc.net`

Certificate paths:

`/etc/letsencrypt/live/bridging.linkpc.net/fullchain.pem`
`/etc/letsencrypt/live/bridging.linkpc.net/privkey.pem`

Nginx config:


  ```bash
server {
   listen 80;
   server_name bridging.linkpc.net www.bridging.linkpc.net;
   return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name bridging.linkpc.net www.bridging.linkpc.net;

  ssl_certificate /etc/letsencrypt/live/bridging.linkpc.net/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/bridging.linkpc.net/privkey.pem;

  root /var/www/html;
    index index.html index.htm;

  location / {
        try_files $uri $uri/ =404;
    }
}
```
Verified with nginx -t and external SSL checkers.

###3c Automation

Certbot timer:

`systemctl status certbot.timer`

Manual test:

`sudo certbot renew` --dry-run

Renewal logs: 

`/var/log/letsencrypt/letsencrypt.log.`

Reflection
This was the session that I faced errors the most errors and it was tiring for me to keep changing codes and finding new ways to solve the errors. however, in the end it was worth it when i could see my domain opening. i kept getting error like NET::ERR_CERT_AUTHORITY_INVALID because my network used Fortinet SSL inspection. I have an issue creating my domain on Freenom, which was recommended; it was out of service, and all other websites that create domains require us to pay for their service. Finally i found a website called Freedomain.one. Next to that, the Certbot error started to show up as it couldn’t reach my server on port 80/443 to verify the domain. So i had to go to the AWS web and edit my inbound security as i had to add in the http and https. This opens up the port 443 and port 80. After that, I accidentally edited my SSH service which started to cost SSH error where AWS can’t reach your EC2 instance via SSH. After many attempts, I resolved the issue by changing the IPv4 -IPv6. Then I tried using my.pem key to access the SSH. So I tried to create a new key pair. However, the new key got permission denied. After multiple attempts, i decided to start by delelting my old instance and a new onw with a new ip. with the New instance, it was much easier for me.  i also had a addition step where i had to change the pubic ip of my domain. 
At first, when i change my sercurity I confirmed externally that my certificate was valid, which taught me the importance of testing from multiple perspectives. I also learned how automation ensures certificates renew without manual effort.

### Session 4 – Consulting Simulation & Final Reflection

4a Consulting Simulation
- Participated in Q&A with teaching staff.
- Issues I raised:
- Typo in Certbot package.
- SSL interception by Fortinet.
- Missing ssl_certificate lines in Nginx.
- Optional service explored: MySQL
sudo apt install mysql-server -y
sudo mysql_secure_installation
mysql -u root -p
CREATE DATABASE testdb;


4b Finalisation & Feedback
Compiled README with all sessions.
- Added screenshots of DNS, Certbot, Nginx, SSL checkers, and MySQL.
- Practised explaining my work as if I were a consultant.
- Reflected on peer demos and feedback.

Reflection
I realised that consulting is not just about technical skills but also about communication. Explaining why Fortinet caused SSL errors helped me practice client‑style articulation. Exploring MySQL gave me confidence to try new services. Overall, I grew from a student experimenting with commands into someone who can deploy, secure, and explain a cloud service professionally.

i have added my video in my reflection doc as i was't able to acess the sharepoint the teacher shared


