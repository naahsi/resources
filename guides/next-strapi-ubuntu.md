# Next.js x Strapi on an Ubuntu 20.04 server

## Set up the server
*Condensed from [DigitalOcean's documents](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04)*

### Create a user

1. Log in as root `ssh -i ~/path/to/key root@server_ip`
2. Create a new user `adduser username`
3. Grant administrative privileges `usermod -aG sudo username`

### Set up a firewall

1. Allow OpenSSH `ufw allow OpenSSH`
2. Enable the firewall `ufw enable`
3. Check if it's enabled `ufw status`

### Allow external access for your user

1. Copy `root` user's SSH directory and permissions 
```
rsync --archive --chown=username:username ~/.ssh /home/username
```
2. Test that it works in a new terminal window `ssh -i ~/path/to/key username@server_ip`

## Install & configure Node.js
*Condensed from [Strapi's documents](https://docs.strapi.io/developer-docs/latest/setup-deployment-guides/deployment/hosting-guides/digitalocean.html#_6-you-will-install-node-js)*

1. Install [desired version](https://github.com/nodesource/distributions/blob/master/README.md) `curl -fsSL https://rpm.nodesource.com/node_version.x | bash -`
2. Create a global directory for Node.js modules `mkdir ~/.npm-global`
3. Amend NPM configuration `npm config set prefix '~/.npm-global'`
4. Create or modify shell profile `sudo nano ~/.profile`
5. Add global directory to path by pasting this in the file `export PATH=~/.npm-global/bin:$PATH`
6. Reload the profile `source ~/.profile`

## Install & configure NGINX
*Condensed from [DigitalOcean's docs](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04)

1. Make sure `apt` is up-to-date `sudo apt update`
2. Install NGINX `sudo apy install nginx`
3. Enable external, unencrypted traffic `sudo ufw allow 'Nginx HTTP'`
4. Make a new directory for your server block `sudo mkdir -p /var/www/domain_name/html`
5. Assign ownership to the directory `sudo chown -R $USER:$USER /var/www/domain_name/html`
6. Create a configuration file for the block `sudo nano /etc/nginx/sites-available/domain_name`
7. Add some basic configuration to the file
```
server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }
}
```
8. Enable the configuration `sudo ln -s /etc/nginx/domain_name /etc/nginx/sites-enabled/`
9. Make sure you'll have enough hash bucket memory by uncommenting the following line in `/etc/nginx/nginx.conf`
```
...
http {
  ...
  server_names_hash_bucket_size 64;
  ...
}
...
```
10. Check if the configuration is okay `sudo nginx -t`
11. Restart NGINX `sudo systemctl restart nginx`
