# ladder-docker-compose

使用 docker-compose 构建的 V2Ray + Trojan Server + Caddy Web Server。

Trojan 服务端监听 443 端口，对正常来路的 https 请求，Trojan 服务端会转发给 Caddy 服务器处理，返回 Web 页面；
而通过 Trojan 客户端来的请求，则由 Trojan 服务端进行代理，与 V2ray + Websocket + TLS 的原理一样，通过伪装流量，避免被 GFW 提取特征与检测。

V2Ray 服务端采用pandafan的加密方式，使用WebSocket + TLS + Web + 域名伪装模式。

## 使用

克隆本项目并进入项目目录。

1. 编辑 `./caddy/Caddyfile`:
    ```
    example.com:80 {
        root /usr/src/html
        log /usr/src/caddy.log
        index index.html
    }

    example.com:443 {
        root /usr/src/html
        log /usr/src/caddy.log
        index index.html
    }

    example.com:3389 {
        root /usr/src/html
        log /usr/src/caddy.log
        index index.html
    }
    
    ```
   将`example.com`替换成你自己的域名。

2. 编辑 `./trojan/config/config.json`:
   在`config:json:8`位置，将`your_password`替换成你要设置的密码，这是你客户端连接需要用的密码，妥善保管。

   在`config:json:12-13`位置将`example.com` 替换成你自己的域名, 这个路径是 Caddy 自动调用 Let's encrypt 生成的证书路径。
   
3. 编辑 `./v2ray/config/config.json`:
   在`config:json:9`位置，将`a2e56774-1111-1111-1111-1ac984ccdda6`替换成你要设置的VmessId (UUID)。
   
   如果要替换伪装域名，请将自签证书放在 `./wwwroot/ssl` 目录

4. 执行  `docker-compose up`，或者执行`docker-compose up -d`以常驻进程模式运行容器。

5. 如果每个容器都构建完成并没有产生异常退出，那么你的 V2Ray + Trojan + caddy 服务应该已经是正常运转状态了。


## 感谢

部分代码参考项目：https://github.com/FaithPatrick/trojan-caddy-docker-compose
