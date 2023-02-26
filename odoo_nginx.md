To configure Nginx as a reverse proxy for Odoo, you can follow these general steps:

1. Install Nginx on your Ubuntu server:

```
sudo apt update
sudo apt install nginx
```

2. Create a new server block configuration file for Nginx:

```
sudo nano /etc/nginx/sites-available/odoo
```

3. Paste the following configuration into the file:

```
upstream odoo {
    server 127.0.0.1:8069;
}

server {
    listen 80;
    server_name your_domain.com;

    # Redirect HTTP requests to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl;
    server_name your_domain.com;

    ssl_certificate /path/to/your/certificate.crt;
    ssl_certificate_key /path/to/your/private.key;

    ssl_session_timeout 5m;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    proxy_read_timeout 720s;
    proxy_connect_timeout 720s;
    proxy_send_timeout 720s;

    proxy_buffers 16 64k;
    proxy_buffer_size 128k;

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

    location / {
        proxy_pass http://odoo;
    }

    location /longpolling {
        proxy_pass http://odoo;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
    }

    # Static file cache settings
    location ~* /web/static/ {
        proxy_cache_valid 200 60m;
        proxy_buffering on;
        expires 864000;
        proxy_pass http://odoo;
    }
}

```

4. Modify the server_name directive to match your domain name.

5. Modify the ssl_certificate and ssl_certificate_key directives to point to the SSL certificate and private key files for your domain.

6. Save and close the file.

7. Create a symbolic link to enable the server block configuration:

```
sudo ln -s /etc/nginx/sites-available/odoo /etc/nginx/sites-enabled/odoo
```
8. sudo rm /etc/nginx/sites-enabled/default
```
sudo rm /etc/nginx/sites-enabled/default
```
9. Test the Nginx configuration for syntax errors:

```
sudo nginx -t
```

10. If there are no errors, reload the Nginx configuration:

```
sudo systemctl reload nginx
```

11. Start or restart the Odoo service:

```
sudo systemctl restart odoo
```

12. Open a web browser and navigate to https://your_domain.com to access the Odoo application via Nginx.
