# WPPConnect Team
## _Wppconnect Server Nginx SSL_


## Our online channels

[![Discord](https://img.shields.io/discord/844351092758413353?color=blueviolet&label=Discord&logo=discord&style=flat)](https://discord.gg/JU5JGGKGNG)
[![Telegram Group](https://img.shields.io/badge/Telegram-Group-32AFED?logo=telegram)](https://t.me/wppconnect)
[![WhatsApp Group](https://img.shields.io/badge/WhatsApp-Group-25D366?logo=whatsapp)](https://chat.whatsapp.com/LJaQu6ZyNvnBPNAVRbX00K)
[![YouTube](https://img.shields.io/youtube/channel/subscribers/UCD7J9LG08PmGQrF5IS7Yv9A?label=YouTube)](https://www.youtube.com/c/wppconnect)


Install nginx:

```bash
sudo apt install nginx
```

Remove nginx default site:

```bash
sudo rm /etc/nginx/sites-enabled/default
```

Create a new nginx site to wppconnect-server:

```bash
sudo nano /etc/nginx/sites-available/wppconnet-server
```

Edit and fill it with this information, changing `server_name` to yours equivalent to `myapp.mydomain.com`:

```bash
server {
  server_name myapp.mydomain.com;

  location / {
    proxy_pass http://127.0.0.1:21465;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_cache_bypass $http_upgrade;
  }
}
```

Create a symbolic links to enable nginx sites:

```bash
sudo ln -s /etc/nginx/sites-available/wppconnect-server /etc/nginx/sites-enabled
```

By default, nginx limit body size to 1MB, which isn't enough for some media uploads. Lets change it to 20MB, adding a new line to config file:

```bash
sudo nano /etc/nginx/nginx.conf
...
http {
    ...
    client_max_body_size 20M; # HANDLE BIGGER UPLOADS
}
```

Test nginx configuration and restart server:

```bash
sudo nginx -t
sudo service nginx restart
```

Now, enable SSL (https) on your sites to use all app features like notifications and sending audio messages. An easy way to this is using Certbot:

Install certbot:

```bash
sudo snap install --classic certbot
sudo apt update
```

Enable SSL on nginx (Fill / Accept all information required):

```bash
sudo certbot --nginx
```