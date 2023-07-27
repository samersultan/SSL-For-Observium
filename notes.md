MikeN — Today at 4:22 AM
SSL configuration is external to Observium. I would seek out resources for how to configure SSL in Apache if you're setting up an SSL certificate for the first time. If you are a hobbyist, https://letsencrypt.org/ is a popular option, and can even automate the Apache configuration with the certbot app. If it's for serious work stuff, typically your employer will purchase an SSL certificate for you, and the cert provider usually provides instructions on how to configure it in Apache. 


SSL configuration is external to Observium. I would seek out resources for how to configure SSL in Apache if you're setting up an SSL certificate for the first time. If you are a hobbyist, https://letsencrypt.org/ is a popular option, and can even automate the Apache configuration with the certbot app. If it's for serious work stuff, typically your employer will purchase an SSL certificate for you, and the cert provider usually provides instructions on how to configure it in Apache. 
The only change to make in Observium itself would be changing Global Settings > Web UI > External Web URL to use https instead of http, once you have SSL working.
