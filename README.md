# VServer Setup

This is for setup an VServer


## SSH


### 1. create ssh-key pair, for ex. ed25519
```bash
ssh-keygen -t ed25519
```


### 2. login to your server
```bash
ssh <username>@<ip-address-server>
```

enter your **password**

### 3. add the public key to the VM
```bash
ssh-copy-id -i path/to/your/public-key <username>@<ip-address-server>
```

enter your **password**

your public key is added in **.ssh/authorized_keys**

then you can login without password
```bash
ssh -i path/to/your/private-key <username>@<ip-address-server>
```

alternative after first ssh-key login
```bash
ssh <username>@<ip-address-server>
```

### 4. disable Password-Logins

**Make sure the ssh key connection works!**

- Adjust the configuration under `/etc/ssh/sshd_config`
- Find and edit the line `#PasswordAuthentication yes` to that `PasswordAuthentication no`
- Save the file and exit
- Restart the `sshd` service to reload the config changes

```bash
sudo systemctl restart ssh.service
``` 

**Test your changes**
```bash
logout
```

```bash
ssh -o PublicAuthentication=no <username>@<ip-address-server>
```

**IT SHOULD ONLY WORK WITH YOUR SSH KEY!**

```bash
ssh -i path/to/your/private-key <username>@<ip-address-server>
```

## Nginx

### Install Nginx
```bash
sudo apt update
sudo apt install nginx -y
```

### status from Nginx
```bash
systemctl status nginx.service
```

Go to your browser and write in the url `<ip-address-server>`

You can see the Nginx default page.

### Config webserver
#### 1. create a new file `alternate-index.html` in the location `/var/www/alternatives/`
- Ensure this directory exists by running `ls /var/www/`
- Create the directory if it does not yet exist with: `mkdir /var/www/alternatives/`

#### 2. add configuration for the enabled sites for nginx under `/etc/nginx/sites-enabled/`, named `alternatives`
 - run `sudo nano /etc/nginx/sites-enabled/alternatives`
 - add the config from below

```bash
server {
    listen 8081;
    listen [::]:8081;

    root /var/www/alternatives;
    index alternate-index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

#### 3. Restart the `nginx` service with `sudo service nginx restart`
- for controlling status
```bash
systemctl status nginx.service
```
- open your browser with `<ip-address-server>:8081` to see your configured alternative start page for the nginx webserver

#### Default page and config
- 
```bash
# default html
/var/www/html/index.nginx-debian.html

# default webserver config
/etc/nginx/sites-enabled/default

Nginx config
/etc/nginx/nginx.conf
```

