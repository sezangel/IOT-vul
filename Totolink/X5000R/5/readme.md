# Command injection vulnerability in vif_disable function  

Vendor: Totolink  
Version: X5000R (latest version)  
Detail: There is a command injection vulnerability in vif_disable function in mtkwifi.lua  

![image](../image/vif_disable-1.png)
![image](../image/vif_disable-2.png)

We use Burpsuite to attack, the effect is as follows.
![image](../image/vif_disable-3.png)
![image](../image/wget_response.png)
