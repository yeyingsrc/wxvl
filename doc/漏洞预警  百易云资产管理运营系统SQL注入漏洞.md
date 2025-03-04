#  漏洞预警 | 百易云资产管理运营系统SQL注入漏洞   
浅安  浅安安全   2025-03-04 00:04  
  
**0x00 漏洞编号**  
- # CVE-2025-1535  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
百易云资产管理运营系统是一款基于云计算技术的资产管理与运营平台，主要面向各类企事业单位提供全方位的资产管理服务。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SUY4fKhiaBUSVQ3UdXo5USvX8AtN9Q32ptyiaj6bGB9uQt0nia0rX8f9nZjkVcmMxdnKqnMRRDczl39g/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2025-1535**  
  
**漏洞类型：**  
SQL注入  
  
**影响：**  
获取敏感信息  
  
**简述：**  
百易云资产管理运营系统的/wuser/admin.ticket.close.php接口存在SQL注入漏洞，未经身份验证的攻击者可以通过该漏洞获取数据库敏感信息。  
  
**0x04 影响版本**  
- 百易云资产管理运营系统  
  
**0x05****POC状态**  
- 已公开  
  
****  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.baiyishequ.com/  
  
  
  
