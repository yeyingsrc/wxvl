#  漏洞预警 | NAKIVO Backup & Replication任意文件读取漏洞   
浅安  浅安安全   2025-03-04 00:04  
  
**0x00 漏洞编号**  
- # CVE-2024-48248  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
NAKIVO Backup & Replication是一款高效的数据保护解决方案，专为虚拟化、云和物理环境设计。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SXonhb4Z0OQUPUuPpRHZDL9HqgbxF9PKiaibPQwWNy4e3oQc5k9lBCHwheNXicZGk9oQfoGAnNINxZibg/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2024-48248**  
  
**漏洞类型：**  
任意文件读取  
  
**影响：**  
窃取敏感信息  
  
**简述：**  
NAKIVO Backup & Replication的/c/router接口存在任意文件读取漏洞，未经身份验证的攻击者可以通过该漏洞读取服务器任意文件，从而获取大量敏感信息。  
  
**0x04 影响版本**  
- NAKIVO Backup & Replication <= 10.11.3.86570  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.nakivo.com/  
  
  
  
