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
