# Command injection vulnerability in vif_enable function  

Vendor: Totolink  
Version: X5000R (latest version)  
Detail: There is a command injection vulnerability in vif_enable function in mtkwifi.lua  

![image](../image/vif_enable-1.png)
![image](../image/vif_enable-2.png)

We use Burpsuite to attack, the effect is as follows.
![image](../image/vif_enable-3.png)
![image](../image/wget_response.png)
