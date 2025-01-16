# Command injection vulnerability in apcli_do_enr_pbc_wps function  

Vendor: Totolink  
Version: X5000R (latest version)  
Detail: There is a command injection vulnerability in apcli_do_enr_pbc_wps function in mtkwifi.lua  

![image](../image/apcli_do_enr_pbc_wps-1.png)
![image](../image/apcli_do_enr_pbc_wps-2.png)

We use Burpsuite to attack, the effect is as follows.
![image](../image/apcli_do_enr_pbc_wps-3.png)
![image](../image/wget_response.png)
