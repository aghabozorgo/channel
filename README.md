{
    "dns": {
        "hosts": {
            "domain:googleapis.cn": "googleapis.com"
        },
        "servers": [
            "1.1.1.1"
        ]
    },
    "inbounds": [
        {
            "listen": "127.0.0.1",
            "port": 10808,
            "protocol": "socks",
            "settings": {
                "auth": "noauth",
                "udp": true,
                "userLevel": 8
            },
            "sniffing": {
                "destOverride": [
                    "http",
                    "tls"
                ],
                "enabled": true
            },
            "tag": "socks"
        },
        {
            "listen": "127.0.0.1",
            "port": 10809,
            "protocol": "http",
            "settings": {
                "userLevel": 8
            },
            "tag": "http"
        }
    ],
    "log": {
        "loglevel": "warning"
    },
    "outbounds": [
        {
            "mux": {
                "concurrency": -1,
                "enabled": false,
                "xudpConcurrency": 8,
                "xudpProxyUDP443": ""
            },
            "protocol": "vless",
            "settings": {
                "vnext": [
                    {
                        "address": "95.164.84.87",
                        "port": 443,
                        "users": [
                            {
                                "encryption": "none",
                                "flow": "",
                                "id": "a8f1f548-2d45-4ba2-8ec0-082aefebff9f",
                                "level": 8,
                                "security": "auto"
                            }
                        ]
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp",
                "security": "",
                "tcpSettings": {
                    "header": {
                        "request": {
                            "headers": {
                                "Connection": [
                                    "keep-alive"
                                ],
                                "Host": [
                                    "ponisha.ir"
                                ],
                                "Pragma": "no-cache",
                                "Accept-Encoding": [
                                    "gzip, deflate"
                                ],
                                "User-Agent": [
                                    "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.143 Safari/537.36",
                                    "Mozilla/5.0 (iPhone; CPU iPhone OS 10_0_2 like Mac OS X) AppleWebKit/601.1 (KHTML, like Gecko) CriOS/53.0.2785.109 Mobile/14A456 Safari/601.1.46"
                                ]
                            },
                            "method": "GET",
                            "path": [
                                "/"
                            ],
                            "version": "1.1"
                        },
                        "type": "http"
                    }
                },
                "sockopt": {
                    "dialerProxy": "fragment",
                    "tcpKeepAliveIdle": 100,
                    "tcpNoDelay": true
                }
            },
            "tag": "proxy"
        },
        {
            "tag": "fragment",
            "protocol": "freedom",
            "settings": {
                "domainStrategy": "AsIs",
                "fragment": {
                    "packets": "tlshello",
                    "length": "100-200",
                    "interval": "10-20"
                }
            },
            "streamSettings": {
                "sockopt": {
                    "tcpKeepAliveIdle": 100,
                    "tcpNoDelay": true
                }
            }
        },
        {
            "protocol": "freedom",
            "settings": {},
            "tag": "direct"
        },
        {
            "protocol": "blackhole",
            "settings": {
                "response": {
                    "type": "http"
                }
            },
            "tag": "block"
        }
    ],
    "routing": {
        "domainStrategy": "IPIfNonMatch",
        "rules": [
            {
                "ip": [
                    "1.1.1.1"
                ],
                "outboundTag": "proxy",
                "port": "53",
                "type": "field"
            }
        ]
    }
}
