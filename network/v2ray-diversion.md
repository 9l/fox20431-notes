# v2ray流量分流

参考文章：https://toutyrater.github.io/routing/configurate_rules.html

```json
{
  "inbounds": [{
    "port": 10808,  // 本地端口，更具喜好更改
    "listen": "127.0.0.1",  // 环回地址，一般不要更改
    "protocol": "socks",
    "settings": {
      "udp": true
    }
  }],
  "outbounds": [{
    "protocol": "vmess",
    "settings": {
      "vnext": [{
        "address": "ip or domain name", // 服务器IP地址或者域名
        "port": 10086,  // 服务器v2ray开放的指定端口
        "users": [{ "id": "uuid" }] // 服务端v2ray生成的入口的uuid
      }]
    }
  },{
    "protocol": "freedom",
    "tag": "direct",
    "settings": {}
  }],
  // 分流规则
  "dns": {
    "servers": [
      "114.114.114.114",
      {
        "address": "1.1.1.1",
        "port": 53,
        "domains": [
          "geosite:geolocation-!cn"
        ]
      }
    ]
  },
  "routing": {
    "domainStrategy": "IPOnDemand",
    "rules": [
      {
        "type": "field",
        "outboundTag": "direct",
        "domain": ["geosite:cn"]  // 中国大陆网址
      },
      {
        "type": "field",
        "outboundTag": "direct", 
        "ip": [
          "geoip:cn", // 中国大陆IP
          "geoip:private"
        ]
      },
      {
        "type": "field",
        "outboundTag": "proxy",
        "network": "udp,tcp"
      }  
    ]
  }
}
```

测试配置文件
`v2ray -test -config=/usr/local/etc/v2ray/config.json`

重启客户端v2ray