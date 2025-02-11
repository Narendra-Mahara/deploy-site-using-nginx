# Hosting a Static Website on Ubuntu using Nginx

## Step 1: Install Nginx

```bash
sudo apt update
sudo apt install nginx -y
```

- To verify Nginx is running, open your browser and enter your server's IP. You should see the default Nginx welcome page.

## Step 2: Configure Firewall (if not enabled)

```bash
sudo ufw allow 'Nginx Full'
sudo ufw allow 22/tcp
sudo ufw enable
```

- This ensures both HTTP (port 80) and HTTPS (port 443) are accessible.

## Step 3: Deploy Your Static Website

```bash
cd /var/www/html
sudo rm index.nginx-debian.html  # Remove the default Nginx page
```

- Now, upload or clone your static site into `/var/www/html`.

## Step 4: Configure Nginx Virtual Host

```bash
sudo nano /etc/nginx/sites-available/default
```

Modify the file to include:

```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

- Save and exit (`CTRL + X`, then `Y`, then `Enter`).
- Restart Nginx to apply changes:

```bash
sudo systemctl restart nginx
```

## Step 5: Set Up SSL with Certbot (if you have a domain)

### Install Certbot:

```bash
sudo apt install certbot python3-certbot-nginx -y
```

### Obtain SSL Certificate:

```bash
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

### Test Auto-Renewal:

```bash
sudo certbot renew --dry-run
```

## Notes:

- If you do not have a domain, you can access your site via your server's IP.
- Ensure your domain's DNS has the following **A records** pointing to your server's IP:
  - `@` → `your_server_ip`
  - `www` → `your_server_ip`
- If you make changes to the Nginx config, always test before restarting:

```bash
sudo nginx -t
```

