# About this Project
This documentation refers to installation of Certbot or HTTPS for HTTP application, Purpose to implement this is to secure communication between the Client with your web server

Before proceed you have nginx ready on your machine, because it will act as web server for your app.

- Install nginx
```sh
sudo apt install nginx -y 
```

To enable TLS we'll use Free certificate provided by Let's encrypt. 

1. Install certbot: certbot is a tool that used to automatically install a free SSL certificate from let's encrypt
```sh
sudo apt install certbot python3-certbot-nginx -y
```

2. we have to raise the request to get the certificate
```sh
sudo certbot nginx -d your-domain.com -d www.your-domain.com
```
once you run this command you will ask for an email. afterward your http will become https.

3. renew your certificate
check status of the certbot service
```sh
sudo systemctl status certbot.timer
```
the output 
```sh
● certbot.timer - Run certbot twice daily
     Loaded: loaded (/lib/systemd/system/certbot.timer; enabled; vendor preset: enabled)
     Active: active (waiting) since Sun 2026-01-18 20:46:36 WIB; 24min ago
    Trigger: Mon 2026-01-19 11:43:24 WIB; 14h left
   Triggers: ● certbot.service

Jan 18 20:46:36 yogaw systemd[1]: Started Run certbot twice daily.
```
inside the output there's a **``Run certbot twice daily``** this is a cronjob that check if the renewale is require or not.

always renew your certificate automatically with this command
```sh
sudo certbot renew --dry-run
```