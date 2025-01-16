# Command injection vulnerability in apcli_wps_gen_pincode function  

Vendor: Totolink  
Version: X5000R (latest version)  
Detail: There is a command injection vulnerability in apcli_wps_gen_pincode function in mtkwifi.lua  

![image](../image/apcli_wps_gen_pincode-1.png)
![image](../image/apcli_wps_gen_pincode-2.png)

We use Burpsuite to attack, the effect is as follows.
![image](../image/apcli_wps_gen_pincode-3.png)
![image](../image/wget_response.png)
