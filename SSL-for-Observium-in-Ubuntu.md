To enable HTTPS on an Observium installation in Ubuntu, you'll typically need to use a web server like Apache or Nginx as a reverse proxy and configure it with an SSL certificate. Here's a step-by-step guide using Let's Encrypt and Apache:

1. **Install Apache** if it's not already installed:

```bash
sudo apt update
sudo apt install apache2
```

2. **Enable the necessary Apache modules** for SSL and proxying:

```bash
sudo a2enmod ssl
sudo a2enmod proxy
sudo a2enmod proxy_http
```

3. **Obtain an SSL certificate** from Let's Encrypt:

First, install the Let's Encrypt client (certbot):

```bash
sudo apt install certbot python3-certbot-apache
```

Then, obtain and install an SSL certificate by running:

```bash
sudo certbot --apache -d yourdomain.com
```

Replace `yourdomain.com` with your actual domain name. Follow the on-screen prompts to complete the process. Certbot will take care of the Apache SSL configuration.

4. **Configure Apache as a reverse proxy** for Observium:

Create a new configuration file:

```bash
sudo nano /etc/apache2/sites-available/observium.conf
```

Add the following content to the file:

```apache
<VirtualHost *:443>
    ServerName yourdomain.com
    ServerAdmin admin@yourdomain.com

    ProxyPreserveHost On
    ProxyPass / http://localhost:your_observium_port/
    ProxyPassReverse / http://localhost:your_observium_port/

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/yourdomain.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/yourdomain.com/privkey.pem
</VirtualHost>
```

Replace `yourdomain.com` with your actual domain name and `your_observium_port` with the port Observium is running on.

Save and close the file.

5. **Enable the new site configuration**:

```bash
sudo a2ensite observium
```

6. **Restart Apache** to apply the changes:

```bash
sudo systemctl restart apache2
```

Now, your Observium instance should be accessible via HTTPS at `https://yourdomain.com`.

Remember to replace "yourdomain.com" and "your_observium_port" with your actual domain and port number. Note that you need to have a valid domain name pointed to your server for Let's Encrypt to work.

Also, make sure your server's firewall settings allow traffic on port 443. If you're using `ufw`, you can do that with:

```bash
sudo ufw allow 443
```

Please note that this is a high-level guide and actual steps can vary based on the specifics of your setup, like the version of Ubuntu, Apache, Observium, etc.
