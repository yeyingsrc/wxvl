#  越权漏洞检测的工具 - PrivHunterAI   
 GSDK安全团队   2025-03-03 12:22  
  
01 项目地址  
  
```
https://github.com/Ed1s0nZ/PrivHunterAI
```  
  
  
  
02 项目介绍  
  
功能  
  
一款通过被动代理方式调用Kimi、DeepSeek和通义千问AI，实现越权漏洞检测的工具。检测能力基于对应AI引擎的API实现，且支持HTTPS协议。  
  
工作流程  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Xu1xJEZRrFhvXl8YZohA5km4WfAHJ5J2jmpIv11fgHZfAUHn64aSaegEkuc0iaHibK8KAicssEo602cvBkLs1v0Fg/640?wx_fmt=png&from=appmsg "")  
  
使用方法  
- 下载源代码；  
  
- 编辑根目录下的config.json文件，配置AI和对应的apiKeys（只需要配置一个即可）；（AI的值可配置qianwen、kimi 或 deepseek） ；  
  
- 配置headers2（请求B对应的headers）；可按需配置suffixes、allowedRespHeaders（接口后缀白名单，如.js）；  
  
- 执行go build编译项目，并运行二进制文件；  
  
- 首次启动后需安装证书以解析 HTTPS 流量，证书会在首次启动命令后自动生成，路径为 ~/.mitmproxy/mitmproxy-ca-cert.pem。安装步骤可参考 Python mitmproxy 文档：About Certificates。  
  
- BurpSuite 挂下级代理 127.0.0.1:9080（端口可在mitmproxy.go 的Addr:":9080", 中配置）即可开始扫描；  
  
- 终端和web界面均可查看扫描结果，前端查看结果请访问127.0.0.1:8222 。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Xu1xJEZRrFhvXl8YZohA5km4WfAHJ5J2w8BnEiactustFNbSY2Vs7DLzTGxo4gjsozStTPJtd1EJA5XZSpfOuTA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Xu1xJEZRrFhvXl8YZohA5km4WfAHJ5J2ftPRBhbNKJ5XKiaOJPEQTk3Ck1Hfdudx1HHbO3NaWy9ywDtYDQQuK1Q/640?wx_fmt=png&from=appmsg "")  
  
注：  
工具仅供安全研究与学习之用，若将工具做其他用途，由使用者承担全部法律及连带责任，作者及发布者不承担任何法律及连带责任。信息及工具收集于互联网，真实性及安全性自测！！  
  
  
