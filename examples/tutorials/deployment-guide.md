# How to Deploy a Node.js Application to AWS EC2

> Complete step-by-step guide for deploying a production-ready Node.js application to Amazon EC2 with NGINX, SSL, and PM2

**Difficulty:** Intermediate  
**Time Required:** 45 minutes  
**Cost:** Free tier eligible  
**Last Updated:** October 31, 2025

---

## Prerequisites

Before starting this tutorial, ensure you have:

- ✅ An AWS account with EC2 access
- ✅ Basic knowledge of command line/terminal
- ✅ A Node.js application ready to deploy
- ✅ (Optional) A domain name for custom URL
- ✅ SSH client installed on your computer

---

## What You'll Learn

By the end of this tutorial, you will:

1. Launch and configure an EC2 instance
2. Set up Node.js and required dependencies
3. Deploy your application with PM2 for process management
4. Configure NGINX as a reverse proxy
5. Add SSL certificate for HTTPS
6. Implement basic security measures

---

## Step 1: Launch an EC2 Instance

### 1.1 Access EC2 Console

1. Log in to [AWS Console](https://console.aws.amazon.com/)
2. Navigate to **Services** → **EC2**
3. Click **Launch Instance**

### 1.2 Configure Instance

**Choose Amazon Machine Image (AMI):**
- Select **Ubuntu Server 22.04 LTS (HVM), SSD Volume Type**
- Architecture: **64-bit (x86)**

**Choose Instance Type:**
- Select **t2.micro** (free tier eligible)
- 1 vCPU, 1 GB RAM is sufficient for small applications

**Configure Instance Details:**
- Keep default settings
- Enable **Auto-assign Public IP**

**Add Storage:**
- Default 8 GB is usually enough
- Increase to 16-30 GB for larger applications

### 1.3 Configure Security Group

Create a new security group with these inbound rules:

| Type | Protocol | Port Range | Source | Description |
|------|----------|------------|--------|-------------|
| SSH | TCP | 22 | My IP | SSH access |
| HTTP | TCP | 80 | Anywhere | HTTP traffic |
| HTTPS | TCP | 443 | Anywhere | HTTPS traffic |

> **Security Tip:** Restrict SSH access to your IP address only. Never use "Anywhere" (0.0.0.0/0) for SSH in production.

### 1.4 Create Key Pair

1. Click **Create a new key pair**
2. Name it descriptively (e.g., `my-app-production`)
3. Choose **RSA** and **.pem** format
4. **Download** the key pair file
5. Click **Launch Instances**

**Save your key pair securely!** You cannot download it again.

---

## Step 2: Connect to Your Instance

### 2.1 Set Key Permissions (Mac/Linux)

```bash
# Navigate to where you downloaded the key
cd ~/Downloads

# Set correct permissions (required for SSH)
chmod 400 my-app-production.pem
```

### 2.2 Get Instance Public IP

1. Go to EC2 Dashboard
2. Click **Instances**
3. Select your instance
4. Copy the **Public IPv4 address** (e.g., 54.123.45.67)

### 2.3 Connect via SSH

```bash
ssh -i my-app-production.pem ubuntu@YOUR_INSTANCE_IP

# Example:
ssh -i my-app-production.pem ubuntu@54.123.45.67
```

If you see a warning about authenticity, type `yes` to continue.

**Windows Users:** Use [PuTTY](https://www.putty.org/) or Windows Terminal with OpenSSH.

---

## Step 3: Update System and Install Node.js

### 3.1 Update Package Lists

```bash
# Update package index
sudo apt update

# Upgrade installed packages
sudo apt upgrade -y
```

### 3.2 Install Node.js 20.x

```bash
# Add NodeSource repository
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Install Node.js and npm
sudo apt-get install -y nodejs

# Verify installation
node --version  # Should show v20.x.x
npm --version   # Should show 10.x.x
```

### 3.3 Install Additional Dependencies

```bash
# Install build essentials (needed for some npm packages)
sudo apt install -y build-essential

# Install Git
sudo apt install -y git

# Verify Git installation
git --version
```

---

## Step 4: Deploy Your Application

### 4.1 Clone Your Repository

```bash
# Navigate to home directory
cd ~

# Clone your repository
git clone git@github.com:hfourtech/geo_boilder_plate.git

# Navigate to app directory
cd your-app
```

**Don't have your code in Git?** Use `scp` to copy files:

```bash
# From your local machine:
scp -i my-app-production.pem -r ./your-app ubuntu@YOUR_INSTANCE_IP:~/
```

### 4.2 Install Dependencies

```bash
# Install production dependencies only
npm install --production

# Or if you have a lockfile:
npm ci --production
```

### 4.3 Configure Environment Variables

```bash
# Create .env file
nano .env
```

Add your environment variables:

```env
NODE_ENV=production
PORT=3000
DATABASE_URL=your_database_connection_string
JWT_SECRET=your_secret_key_here
API_KEY=your_api_key_here
```

**Save and exit:** Press `Ctrl + X`, then `Y`, then `Enter`

> **Security Note:** Never commit .env files to Git. Add them to .gitignore.

### 4.4 Test Your Application

```bash
# Run your app temporarily
npm start

# Or if you use a different command:
node server.js
```

Open another terminal and test:

```bash
curl http://YOUR_INSTANCE_IP:3000
```

If you see your app's response, it's working! Press `Ctrl + C` to stop.

---

## Step 5: Install and Configure PM2

PM2 is a production process manager that keeps your Node.js app running continuously.

### 5.1 Install PM2 Globally

```bash
sudo npm install -g pm2
```

### 5.2 Start Your Application

```bash
# Start your app with PM2
pm2 start npm --name "your-app" -- start

# Or if you have a specific entry file:
pm2 start server.js --name "your-app"

# For apps that need environment variables:
pm2 start server.js --name "your-app" --env production
```

### 5.3 Configure Auto-Restart on Boot

```bash
# Generate startup script
pm2 startup systemd

# PM2 will output a command like this - run it:
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u ubuntu --hp /home/ubuntu

# Save PM2 process list
pm2 save
```

### 5.4 Useful PM2 Commands

```bash
# View all running processes
pm2 list

# View logs
pm2 logs your-app

# View real-time monitoring
pm2 monit

# Restart app
pm2 restart your-app

# Stop app
pm2 stop your-app

# Delete app from PM2
pm2 delete your-app
```

---

## Step 6: Install and Configure NGINX

NGINX acts as a reverse proxy, forwarding requests from port 80/443 to your Node.js app on port 3000.

### 6.1 Install NGINX

```bash
sudo apt install nginx -y

# Start NGINX
sudo systemctl start nginx

# Enable NGINX to start on boot
sudo systemctl enable nginx

# Check status
sudo systemctl status nginx
```

### 6.2 Configure NGINX Server Block

```bash
# Create configuration file
sudo nano /etc/nginx/sites-available/your-app
```

Add this configuration:

```nginx
server {
    listen 80;
    listen [::]:80;
    
    # Replace with your domain or use _ for IP-only access
    server_name yourdomain.com www.yourdomain.com;
    
    location / {
        # Forward requests to Node.js app
        proxy_pass http://localhost:3000;
        
        # Preserve original host and IP
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

**Save and exit** (`Ctrl + X`, `Y`, `Enter`)

### 6.3 Enable Configuration

```bash
# Create symbolic link to enable site
sudo ln -s /etc/nginx/sites-available/your-app /etc/nginx/sites-enabled/

# Remove default NGINX page (optional)
sudo rm /etc/nginx/sites-enabled/default

# Test NGINX configuration
sudo nginx -t

# If test passes, restart NGINX
sudo systemctl restart nginx
```

### 6.4 Test Your Deployment

Visit your instance's public IP in a browser:

```
http://YOUR_INSTANCE_IP
```

You should see your application running! 🎉

---

## Step 7: Configure SSL with Let's Encrypt (Optional)

Add free HTTPS encryption to your application.

### 7.1 Prerequisites

- A domain name pointing to your EC2 instance's IP address
- DNS A record configured (wait 10-30 minutes for propagation)

### 7.2 Install Certbot

```bash
# Install Certbot and NGINX plugin
sudo apt install certbot python3-certbot-nginx -y
```

### 7.3 Obtain SSL Certificate

```bash
# Replace with your actual domain
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

Follow the prompts:

1. Enter your email address
2. Agree to terms of service (press Y)
3. Choose whether to redirect HTTP to HTTPS (recommended: 2)

Certbot will automatically:
- Obtain SSL certificate
- Configure NGINX for HTTPS
- Set up auto-renewal

### 7.4 Test Auto-Renewal

```bash
# Dry run to test renewal
sudo certbot renew --dry-run
```

If successful, certificates will auto-renew before expiration.

### 7.5 Verify HTTPS

Visit your site with HTTPS:

```
https://yourdomain.com
```

You should see a secure padlock icon! 🔒

---

## Step 8: Configure Firewall

Add an extra layer of security with UFW (Uncomplicated Firewall).

### 8.1 Set Up UFW

```bash
# Allow SSH (IMPORTANT: do this first!)
sudo ufw allow OpenSSH

# Allow HTTP and HTTPS
sudo ufw allow 'Nginx Full'

# Enable firewall
sudo ufw enable

# Verify status
sudo ufw status
```

Output should show:

```
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx Full                 ALLOW       Anywhere
```

---

## Step 9: Monitoring and Maintenance

### 9.1 View Application Logs

```bash
# View PM2 logs
pm2 logs your-app

# View last 100 lines
pm2 logs your-app --lines 100

# View error logs only
pm2 logs your-app --err
```

### 9.2 Monitor System Resources

```bash
# View real-time process monitor
pm2 monit

# Check disk usage
df -h

# Check memory usage
free -h

# Check running processes
top
```

### 9.3 Update Your Application

```bash
# Navigate to app directory
cd ~/your-app

# Pull latest code
git pull origin main

# Install new dependencies
npm install --production

# Restart app with zero downtime
pm2 reload your-app
```

---

## Common Issues and Troubleshooting

### Issue: Cannot Connect via SSH

**Solution:**
- Check security group allows SSH from your IP
- Verify key file permissions: `chmod 400 your-key.pem`
- Ensure using correct username: `ubuntu` for Ubuntu AMI

### Issue: Application Not Accessible

**Solution:**
```bash
# Check if app is running
pm2 list

# Check app logs
pm2 logs your-app

# Check NGINX status
sudo systemctl status nginx

# Check NGINX error logs
sudo tail -f /var/log/nginx/error.log
```

### Issue: Port 3000 Already in Use

**Solution:**
```bash
# Find process using port 3000
sudo lsof -i :3000

# Kill the process (replace PID with actual number)
kill -9 PID
```

### Issue: SSL Certificate Not Working

**Solution:**
- Verify DNS is pointing to correct IP
- Wait for DNS propagation (up to 48 hours)
- Check Certbot logs: `sudo tail -f /var/log/letsencrypt/letsencrypt.log`

---

## Next Steps

Now that your application is deployed, consider:

1. **Set up monitoring:** Use PM2 Plus, New Relic, or Datadog
2. **Configure automatic backups:** Use AWS Snapshots
3. **Add a CDN:** CloudFront or Cloudflare for static assets
4. **Implement CI/CD:** Automate deployments with GitHub Actions
5. **Scale horizontally:** Use Load Balancers for multiple instances
6. **Set up logging:** Centralize logs with CloudWatch or ELK stack

---

## Estimated Costs

**EC2 t2.micro instance:**
- Free tier: 750 hours/month for 12 months
- After free tier: ~$8-10/month

**Data transfer:**
- First 1 GB/month free
- After: $0.09/GB

**Domain name (optional):**
- $10-15/year

**SSL Certificate:**
- Free with Let's Encrypt

**Total estimated cost:** $10-15/month after free tier

---

## Additional Resources

- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [PM2 Documentation](https://pm2.keymetrics.io/docs/)
- [NGINX Documentation](https://nginx.org/en/docs/)
- [Let's Encrypt Documentation](https://letsencrypt.org/docs/)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)

---

## Conclusion

Congratulations! You've successfully deployed a production-ready Node.js application to AWS EC2 with:

✅ Automatic process management with PM2  
✅ Reverse proxy with NGINX  
✅ SSL encryption with Let's Encrypt  
✅ Basic security configuration  
✅ Monitoring and logging setup

Your application is now accessible 24/7 with automatic restarts and secure HTTPS connections.

---

**Questions or issues?** Open an issue on [GitHub](https://github.com/yourusername/geo-guide) or reach out on [Twitter](https://twitter.com/yourhandle).

**Found this helpful?** Share it with other developers!

---

*Last updated: October 31, 2025 • Written by Your Name*