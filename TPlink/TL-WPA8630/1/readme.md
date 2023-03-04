**There is a cmd injection vulnerability in httpd of TL-WPA8630**  
**Verision:TL-WPA8630 KIT(US)_V2_171011**

The function sub_40A918 deal the /admin/powerline and call the function sub_40A918, it use sub_40A774 deal the key parameter without checking. It can result in cmd injection.

![image](https://github.com/sezangel/IOT-vul/blob/main/TPlink/TL-WPA8630/image/10.png)
![image](https://github.com/sezangel/IOT-vul/blob/main/TPlink/TL-WPA8630/image/11.png)
![image](https://github.com/sezangel/IOT-vul/blob/main/TPlink/TL-WPA8630/image/12.png)
![image](https://github.com/sezangel/IOT-vul/blob/main/TPlink/TL-WPA8630/image/13.png)
![image](https://github.com/sezangel/IOT-vul/blob/main/TPlink/TL-WPA8630/image/14.png)
