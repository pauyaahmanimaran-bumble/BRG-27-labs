# BRG-27-labs
lab activities 
git clone https://github.com/pauyaahmanimaran-bumble/BRG-27-labs.git
cd BRG-27-labs
Session 3 – DNS, SSL, and Automation

3a DNS Setup

Root domain bridging.linkpc.net → EC2 public IP 3.27.10.254.

Subdomain www.bridging.linkpc.net included via SAN.

Verified with nslookup and external SSL checkers.

3b SSL Certificate & Nginx

Installed Certbot:

sudo apt install certbot python3-certbot-nginx -y

Issued certificate:

sudo certbot --nginx -d bridging.linkpc.net -d www.bridging.linkpc.net

Certificate paths:

/etc/letsencrypt/live/bridging.linkpc.net/fullchain.pem
/etc/letsencrypt/live/bridging.linkpc.net/privkey.pem

Nginx config:

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

Verified with nginx -t and external SSL checkers.

3c Automation

Certbot timer:

systemctl status certbot.timer

Manual test:

sudo certbot renew --dry-run

Renewal logs: /var/log/letsencrypt/letsencrypt.log.

Deploy hook ensures Nginx reloads after renewal.

Session 4a – Consulting Simulation & Reflection

Q&A Issues Raised

Typo in Certbot package name.

Browser warning due to Fortinet SSL interception.

Missing ssl_certificate lines in Nginx config.

Resolutions

Correct package installed.

Verified certificate externally.

Updated Nginx config with proper paths.

Optional Service Exploration (MySQL Example)

Installed:

sudo apt install mysql-server -y

Secured with mysql_secure_installation.

Tested with mysql -u root -p.

Created sample database and documented steps.

Session 4b – Finalisation & Feedback

GitHub Documentation

README includes DNS, SSL, Nginx, automation, and MySQL setup.

Screenshots: DNS panel, Certbot success, Nginx test, SSL checker, MySQL login.

Consultant-Style Reflection

Unresolved issue: Fortinet SSL interception.

Clarifications: Certbot auto-renew via systemd.

Practised explaining DNS → SSL → Nginx flow clearly.

Additional Service Exploration

Installed and configured MySQL.

Verified database creation and authentication.

Documented in GitHub.

Reflection

Improvements: Debugging typos, verifying cert chain externally.

Obstacles: Browser mistrust due to Fortinet proxy.

Resolution: External validation confirmed correctness.

Future relevance: Skills applicable to System Admin and DevOps roles.

