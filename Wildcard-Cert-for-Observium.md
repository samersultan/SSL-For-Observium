Using a wildcard certificate with Observium on Ubuntu 20.04 Apache involves a similar process to using a regular SSL certificate, but with a few extra steps. A wildcard certificate allows you to secure all subdomains under a main domain. It's great for situations where you're using multiple subdomains and you want to cover all of them with a single certificate.

Here's how to do it with Let's Encrypt and Apache:

1. **Ensure Apache is installed and the necessary modules are enabled:**

```bash
sudo apt update
sudo apt install apache2
sudo a2enmod ssl
sudo a2enmod proxy
sudo a2enmod proxy_http
```

2. **Install Certbot, the Let's Encrypt client:**

```bash
sudo apt install certbot python3-certbot-apache
```

3. **Request a wildcard certificate:**

Due to the way wildcard certificates are validated, you'll need to use the DNS challenge to obtain the certificate. This involves adding a DNS TXT record to your domain. The command to start the process is:

```bash
sudo certbot certonly --manual --preferred-challenges=dns -d '*.yourdomain.com'
```

Replace `*.yourdomain.com` with your actual domain name.

Follow the prompts on screen. You will be given a DNS TXT record to add to your domain. After adding it, you can continue with the process.

Note: Please consider that Certbot’s `--manual` mode is intended to be interactive, this means that renewal process won't be automated.

4. **Configure Apache as a reverse proxy for Observium:**

Create a new configuration file:

```bash
sudo nano /etc/apache2/sites-available/observium.conf
```

Then, add the following to the new configuration file:

```apache
<VirtualHost *:443>
    ServerName yourdomain.com
    ServerAlias *.yourdomain.com
    ServerAdmin admin@yourdomain.com

    ProxyPreserveHost On
    ProxyPass / http://localhost:your_observium_port/
    ProxyPassReverse / http://localhost:your_observium_port/

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/yourdomain.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/yourdomain.com/privkey.pem
</VirtualHost>
```

Replace `yourdomain.com` with your actual domain name and `your_observium_port` with the port Observium is running on. Save and close the file.

5. **Enable the new site configuration:**

```bash
sudo a2ensite observium
```

6. **Restart Apache to apply the changes:**

```bash
sudo systemctl restart apache2
```

Now, Observium should be accessible via HTTPS at `https://yourdomain.com` and all of its subdomains.

Make sure your firewall allows HTTPS traffic. If you're using UFW, you can allow it with:

```bash
sudo ufw allow 443
```

As always, be sure to replace "yourdomain.com" and "your_observium_port" with your actual domain and port. 

Please note that you need to have a valid domain name pointed to your server for Let's Encrypt to work. And also wildcard certificate will cover all the first-level subdomains but not the second-level or multi-level subdomains (for example, it won't cover `sub.sub.yourdomain.com`). 

This is a high-level guide and actual steps can vary based on the specifics of your setup, like the version of Ubuntu, Apache, Observium, etc.
