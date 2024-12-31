# LoginMyId Installation Steps

1. Download and Install [Cloudflared](https://github.com/cloudflare/cloudflared/releases)
2. Download, extract and add into system path [XTLS X-Ray Core](https://github.com/XTLS/Xray-core/releases) 
3. Run file run.bat
4. Use NekoRay to connect

## Config file

* config.json for Xray-core
* config.yml for Cloudflared

## Light SFTP Server
[Download](https://github.com/loginmyid/scp/releases)

How to use in client:
```sh
scp -P 2022 ./filename x@192.168.1.12:~
```
password : x
