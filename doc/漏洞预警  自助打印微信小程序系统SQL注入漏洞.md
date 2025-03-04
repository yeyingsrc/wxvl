#  漏洞预警 | 自助打印微信小程序系统SQL注入漏洞   
浅安  浅安安全   2025-03-04 00:04  
  
**0x00 漏洞编号**  
- # 暂无  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
自助打印微信小程序系统支持用户通过小程序上传文件并进行云端打印，方便快捷。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SXokIUEumKMX7rksZVbvbVj2IbvIloGHVlrib1uT6shJLg0oZoVkgQUhdCKdpLw2S0KNe3FyxmcTHg/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞详情**  
###   
  
**漏洞类型：**  
SQL注入  
  
**影响：**  
获取敏感信息  
  
  
  
  
**简述：**  
自助打印微信小程序系统的/api/shop/nearByShop接口存在SQL注入漏洞，未经身份验证的攻击者可以通过该漏洞获取数据库敏感信息。  
  
**0x04 影响版本**  
- 自助打印微信小程序系统  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**暂无官网信息，建议用户禁止该链接对外访问。**  
  
  
  
