# Configure a Domain with Nginx Reverse Proxy and SSL (Let's Encrypt)

## 1. Introduction
This documentation provides step-by-step instructions to configure **any domain** with **Nginx reverse proxy** and secure it using **Let's Encrypt SSL**.  
The SSL certificate will be automatically renewed by Certbot.

---

## 2. Requirements / Prerequisites
- A domain name (e.g., `example.com`)
- A server with **public IP** (where Nginx is installed)
- Nginx installed
- Certbot installed  
  
  ```bash
  sudo apt update
  sudo apt install nginx certbot python3-certbot-nginx -y
  ```
- Backend service running with IP and port (e.g., `http://10.11.8.178:30080`)

---

## 3. DNS Configuration
Point the domain/subdomain to the **public IP** of the Nginx server.

Example DNS record:

```bash
example.yourdomain.com   A   <Public_IP_of_Nginx_Server>
```

---

## 4. SSL Certificate Setup
Generate an SSL certificate with Certbot:

```bash
sudo certbot --nginx -d example.com
```

- Certificate Path: `/etc/letsencrypt/live/example.com/fullchain.pem`  
- Key Path: `/etc/letsencrypt/live/example.com/privkey.pem`  
- Certificate Validity: 90 days  
- Auto-renewal enabled (via cron/systemd timer by Certbot)

---

## 5. Nginx Configuration

### 5.1 Create Server Block for HTTPS
Create a new config file: `/etc/nginx/sites-enabled/example.conf`
```bash
cd /etc/nginx/sites-enabled/
vim example.conf
```
**example.conf** file past here

---

## 6. Verify and Reload Nginx
Test configuration:

```bash
sudo nginx -t
```

If successful, reload Nginx:

```bash
sudo systemctl reload nginx
```

---

## 7. Verify SSL Auto-Renewal
Certbot automatically sets up a cron job or systemd timer for renewal.  
Test renewal with:

```bash
sudo certbot renew --dry-run
```

If successful, SSL will auto-renew every 60 days.

---

## 8. Conclusion
✅ Your domain has been successfully configured with **Nginx reverse proxy** and secured using **Let's Encrypt SSL**.  
🔄 SSL certificate auto-renewal is enabled, ensuring your service always runs over **HTTPS** without downtime.