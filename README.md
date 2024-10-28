# Deployment Documentation for Express API

## Overview
This document outlines the deployment process for the Express API hosted on AWS.

## Services Used
- **Amazon EC2**: Virtual server hosting the Express API.
- **PM2**: Process manager for Node.js applications.
- **NGINX**: Reverse proxy server to handle requests and SSL.
- **Let's Encrypt**: Provides SSL certificates for secure connections.

## Deployment Steps

1. **Launch EC2 Instance**: Set up a `t2.nano` instance running Ubuntu.

2. **Install Node.js and NPM**:
   ```bash
   curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt install nodejs
   ```

3. **Clone the Project**:
   ```bash
   git clone https://github.com/asad2050/e-commerce-backend-aws
   ```

4. **Install Dependencies and Start App**:
   ```bash
   sudo npm install pm2 -g
   cd e-commerce-backend-aws
   pm2 start index
   ```

5. **Configure NGINX**:
   - Go to the root directory and execute the following commands:
   ```bash
   sudo apt install nginx
   sudo nano /etc/nginx/sites-available/default
   ```
   - Add the following to the location part of the server block:
   ```
   server_name yourdomain.com www.yourdomain.com;

   location / {
       proxy_pass http://localhost:8001; # whatever port your app runs on
       proxy_http_version 1.1;
       proxy_set_header Upgrade $http_upgrade;
       proxy_set_header Connection 'upgrade';
       proxy_set_header Host $host;
       proxy_cache_bypass $http_upgrade;
   }
   ```
   ```bash
   # Check NGINX config
   sudo nginx -t

   # Restart NGINX
   sudo nginx -s reload
   ```

6. **Add SSL with Let's Encrypt**:
   ```bash
   sudo certbot --nginx -d domain.com -d www.domain.com
   ```

7. **Domain Setup**:
   - The domain **asadprime.store** was purchased from Hostinger, where the redirect settings were updated to point to the EC2 instance.

## Live Demo

Explore the live Express API using the following links:

- **Primary Demo Link**: [https://api.asadprime.store](https://api.asadprime.store)
- **Alternative Link** (if the primary link is inactive): [https://harmaig-backend.onrender.com](https://harmaig-backend.onrender.com)


## Conclusion
The deployment of the Express API on AWS is complete. For further assistance, feel free to reach out.