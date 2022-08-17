# Update Instance

=> sudo apt update

=> sudo apt --list upgradable

=> sudo apt upgrade

---

# Using Ubuntu | Get node instance (latest stable v16.x)
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -

sudo apt-get install -y nodejs

---

# Upload the files, through FileZilla or Github

## FileZilla Process

=> Write Hostname (Public IP address of instance or EC2 Public DNS)
=> Port 22
=> Optional username = ubuntu
=> Go to Edit, then settings, Under connection, SFTP, Add Key File, add the private key here, click ok.
=> Quickconnect.
=> Optional: Write passphrase if provided.
=> Connection established.

## Github process

=> TODO:

---

# Install all npm files

=> npm install

---

# Install pm2

=> npm install pm2 -g (-g for globally)

# PM2 setup

=> pm2 start server.js (filename)
=> pm2 startup
=> run sudo env script. Will be provided by system after pm2 startup run
=> pm2 save (freeze processes)

---

# Firewall

=> check status => sudo ufw status
=> enable => sudo ufw enable
=> allowSSH => sudo ufw allow ssh
=> allowHTTP => sudo ufw allow http => DO NOT DO THIS IF USING NGINX
=> allowHTTPS > sudo ufw allow https => DO NOT DO THIS IF USING NGINX
=> allowNginxHttp => sudo ufw allow 'Nginx HTTP'
=> allowNginxHttps => sudo ufw allow 'Nginx HTTPS'

---

# Install Oh My Bash

=> bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
=> sudo hostname enter-name-here (anything)
=> exit and connect again for name change

---

# Install Nginx

=> sudo apt install nginx

# Configure NGINX

=> cd /etc/nginx/
=> sudo nano nginx.conf
=> Uncomment server_token in http Basic Settings
=> Add proxy_hide_header X-Powered-By; in Virtual Host configs.

# Check Nginx status

=> sudo nginx -t

# Nginx Sites Available

=> cd /etc/nginx/sites-available
=> sudo nano default
=> Remove all comments
=> change below code for location portion
location / {
proxy_pass http://localhost:5000;
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Host $host;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_cache_bypass $http_upgrade
}
=> change server_name to url address
=> change listen 80 default_server to listen 80
=> listen [::]:80
=> Comment root portion

# Change filename in sites-available

=> sudo mv default <fileName, preferably website name>

# Change nginx sites enabled link to default

=> /etc/nginx/sites-avalable
=> sudo ln -s /etc/nginx/sites-available/<fileName, preferable webiste name> /etc/nginx/sites-enabled/<fileName, preferable website name>
=> What above code means: Please link the sites-available website configured files to my sites-enabled website
=> code meaning: sudo ln -s srcLinkFromPath srcLinkToPath
=> in sites-enabled folder, remove default by sudo rm default

# Nginx check

=> TEST: sudo nginx -t
=> RELOAD: sudo nginx -s reload

---

# In Domain settings

=> Create a new A record, write the host name. Scenario, if root site, write @; if subdomain, write subdomain first, for ex api.example.com, write api. Write points to the static IP in the server.
=> Should be up

---

# Lets Encrypt and Download Snap

Can be found on https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx
=> sudo snap install core
=> sudo snap refresh core (to check updates)
=> sudo apt-get remove certbot (removes any OS installed certbot)
=> sudo snap install --classic certbot (download certbot)
=> sudo ln -s /snap/bin/certbot /usr/bin/certbot (prepare the certbot command)
=> sudo certbot --nginx (get and install certificated); sudo certbot certonly --nginx (only get certificate)
=> Enter email address, Agree Terms of Service, Do not give email address.
=> https enables. check.

# Enable Auto certificate enable

=> sudo certbot renew --dry-run

---

# Done
