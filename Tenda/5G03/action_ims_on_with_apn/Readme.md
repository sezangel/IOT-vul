Submittion Date: 2026.3.16
Vendor: 5G03
Firmware: V05.03.02.04 (Version 1.0)
Download Link: https://www.tenda.com.cn/material/show/4058

In /usr/lib/lua/luci/controller/admin/telephony.lua, the function ```action_ims_on_with_apn``` handles the important parameter string ```ims_apn``` without checking it, 
which leads to a command injection vulnerability. 
