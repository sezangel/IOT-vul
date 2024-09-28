**A stack overflow vulnerability exists in TL-WDR7660**
**version:  TL-WDR7660 ver1.0**

The wlanTimerRuleJsonToBin function is not checked when handling a copy of the important parameter name.It can result in the stack to overflow

![image](https://github.com/sezangel/IOT-vul/tree/main/TPlink/TL-WDR7660/image/1.png)
![image](https://github.com/sezangel/IOT-vul/tree/main/TPlink/TL-WDR7660/image/2.png)
