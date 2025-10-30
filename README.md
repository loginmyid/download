# LoginMyId Installation Steps

1. Download and Install [Cloudflared](https://github.com/cloudflare/cloudflared/releases)
2. Download, extract and add into system path [XTLS X-Ray Core](https://github.com/XTLS/Xray-core/releases) 
3. Run file run.bat
4. Use NekoRay to connect

## Config file

* config.json for Xray-core
  ```json
  {
  "log": { "loglevel": "warning" },
  "inbounds": [
    {
      "port": 10617,
      "listen": "127.0.0.1",
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "08a5d7ec-45d7-4928-9bd6-d9bd97c00cde",
            "alterId": 0
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "path": "/loginmyid",
          "headers": {
            "Host": "your.domain.com"
          }
        },
        "security": "none"
      }
    }
  ],
  "outbounds": [
    { "protocol": "freedom", "settings": {} },
    { "protocol": "blackhole", "settings": {}, "tag": "blocked" }
  ]
  }
  ```
* config.yml for Cloudflared

## Light SFTP Server
[Download](https://github.com/loginmyid/scp/releases)

How to use in client:
```sh
scp -P 2022 ./filename x@192.168.1.12:~
```
password : x

## Nginx reverse proxy

```conf
server {
  listen 443 ssl http2;
  server_name your.domain.com;

  ssl_certificate     /etc/letsencrypt/live/your.domain.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/your.domain.com/privkey.pem;

  location /loginmyid {
    proxy_redirect off;
    proxy_pass http://127.0.0.1:10617;

    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;

    # Optional: timeouts
    proxy_read_timeout 3600s;
    proxy_send_timeout 3600s;
  }
}
```
