# 代理

- v2ray
- privoxy
- proxychains-ng

## v2ray

/etc/v2ray/config.json
```json
{
  "inbounds": [{
    // port
    "port": 10808,
    // localhost
    "listen": "127.0.0.1",
    "protocol": "socks",
    "settings": {
      "udp": true
    }
  }],
  "outbounds": [{
    "protocol": "vmess",
    "settings": {
      "vnext": [{
        // ip
        "address": "example.org",
        // port
        "port": 10086,
        // uuid
        "users": [{ "id": "b495dfdf-3209-4148-8e38-1468fe086c11" }]
      }]
    }
  },{
    "protocol": "freedom",
    "tag": "direct",
    "settings": {}
  }],
  "routing": {
    "domainStrategy": "IPOnDemand",
    "rules": [{
      "type": "field",
      "ip": ["geoip:private"],
      "outboundTag": "direct"
    }]
  }
}
```

## privoxy

将socks代理转换成http代理

`/etc/privoxy/config`尾部添加
```shell
listen-address  :10809
forward-socks5 / 127.0.0.1:10808 .
```

## proxychains-ng

终端代理命令行

`/etc/proxychains.conf`尾部添加
```shell
socks5  127.0.0.1 10808
```