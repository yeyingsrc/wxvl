#  勒索软件团伙利用Paragon分区管理程序漏洞实施BYOVD攻击   
邑安科技  邑安全   2025-03-04 02:54  
  
更多全球网络安全资讯尽在邑安全  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1N39PtINn8uic6EcQDVaB1NszN0c0uq4P6DLjicGrfiahbmFTRmQ7FGdyuGTaT7DcS9WDgjz6qj4ibcsxgyjtf0U6g/640?wx_fmt=png&from=appmsg "")  
  
微软发现Paragon分区管理程序的BioNTdrv.sys驱动存在五个漏洞，其中有一个已被勒索软件团伙用于零日攻击，以获取Windows系统的SYSTEM权限。这些易受攻击的驱动程序在“自带漏洞驱动程序”（BYOVD）攻击中被利用，攻击者将内核驱动程序植入目标系统以提升权限。  
  
**BYOVD攻击原理与影响**  
  
BYOVD攻击的核心在于攻击者可以利用本地设备访问权限，通过漏洞提升权限或导致目标机器拒绝服务（DoS）。CERT/CC的警告指出：“攻击者可以借助微软签名的驱动程序，即使目标系统未安装Paragon分区管理程序，也能通过BYOVD技术实施攻击。”  
  
由于BioNTdrv.sys是一款内核级驱动程序，攻击者可以利用漏洞执行与驱动程序相同权限的命令，从而绕过防护措施和安全软件。微软研究人员发现，其中一个漏洞CVE-2025-0289已被勒索软件团伙用于攻击，尽管具体团伙尚未披露。  
  
CERT/CC公告称：“微软观察到威胁行为体在BYOVD勒索软件攻击中利用这一弱点，特别是通过CVE-2025-0289实现权限提升至SYSTEM级别，随后执行进一步的恶意代码。”Paragon Software已经修复了这些漏洞，微软也通过“漏洞驱动程序阻止列表”阻止了易受攻击的BioNTdrv.sys版本。  
  
**漏洞详情与修复建议**  
  
微软发现的Paragon分区管理程序漏洞包括：  
- CVE-2025-0288：由于“memmove”函数处理不当导致的任意内核内存写入，使攻击者能够写入内核内存并提升权限。  
  
- CVE-2025-0287：由于输入缓冲区中的“MasterLrp”结构缺少验证而导致的空指针解引用，使攻击者能够执行任意内核代码。  
  
- CVE-2025-0286：由于用户提供的数据长度验证不当导致的任意内核内存写入，使攻击者能够执行任意代码。  
  
- CVE-2025-0285：由于未验证用户提供的数据导致的任意内核内存映射，使攻击者能够通过操纵内核内存映射提升权限。  
  
- CVE-2025-0289：由于在将“MappedSystemVa”指针传递给“HalReturnToFirmware”之前未进行验证而导致的内核资源访问不安全，可能导致系统资源被破坏。  
  
前四个漏洞影响Paragon分区管理程序7.9.1及更早版本，而正在被利用的漏洞CVE-2025-0289影响版本17及更早版本。建议用户升级到最新版本，其中包含修复所有漏洞的BioNTdrv.sys 2.0.0版本。  
  
**防护措施与建议**  
  
值得注意的是，即使未安装Paragon分区管理程序的用户也无法幸免于攻击，因为BYOVD攻击并不依赖于目标机器上是否存在该软件。攻击者会将易受攻击的驱动程序与自己的工具一同植入，从而将其加载到Windows系统中以提升权限。  
  
微软已更新“漏洞驱动程序阻止列表”以阻止该驱动程序在Windows中加载。用户和组织应确保启用该防护系统。您可以通过以下路径检查是否启用了阻止列表：**设置→ 隐私与安全→ Windows安全中心→ 设备安全→ 核心隔离→ 微软漏洞驱动程序阻止列表**，并确保该设置已启用。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1N39PtINn8uic6EcQDVaB1NszN0c0uq4PQfZ5AjwHpjC7e5npHb76QbEM1epClFmUOx12nVQFSJ9SoichpopGfyw/640?wx_fmt=png&from=appmsg "")  
  
Windows设置中的漏洞驱动程序阻止列表来源：BleepingComputer  
  
Paragon Software的网站也发布警告，提醒用户必须立即升级Paragon硬盘管理器，因为它使用了相同的驱动程序，而该驱动程序将被微软阻止。  
  
尽管目前尚不清楚哪些勒索软件团伙在利用Paragon漏洞，但BYOVD攻击已越来越受网络犯罪分子欢迎，因为它使他们能够轻松获取Windows设备的SYSTEM权限。已知使用BYOVD攻击的威胁行为体包括Scattered Spider、Lazarus、BlackByte勒索软件、LockBit勒索软件等。  
  
因此，启用微软漏洞驱动程序阻止列表功能以阻止易受攻击的驱动程序在Windows设备上使用显得尤为重要。  
  
原文来自:   
www.bleepingcomputer.com  
  
原文链接: https://www.bleepingcomputer.com/news/security/ransomware-gangs-exploit-paragon-partition-manager-bug-in-byovd-attacks/  
  
欢迎收藏并分享朋友圈，让五邑人网络更安全  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/1N39PtINn8tD9ic928O6vIrMg4fuib48e1TsRj9K9Cz7RZBD2jjVZcKm1N4QrZ4bwBKZic5crOdItOcdDicPd3yBSg/640?wx_fmt=jpeg "")  
  
欢迎扫描关注我们，及时了解最新安全动态、学习最潮流的安全姿势！  
  
推荐文章  
  
1  
  
[新永恒之蓝？微软SMBv3高危漏洞（CVE-2020-0796）分析复现](http://mp.weixin.qq.com/s?__biz=MzUyMzczNzUyNQ==&mid=2247488913&idx=1&sn=acbf595a4a80dcaba647c7a32fe5e06b&chksm=fa39554bcd4edc5dc90019f33746404ab7593dd9d90109b1076a4a73f2be0cb6fa90e8743b50&scene=21#wechat_redirect)  
  
  
2  
  
[重大漏洞预警：ubuntu最新版本存在本地提权漏洞（已有EXP）　](http://mp.weixin.qq.com/s?__biz=MzUyMzczNzUyNQ==&mid=2247483652&idx=1&sn=b2f2ec90db499e23cfa252e9ee743265&chksm=fa3941decd4ec8c83a268c3480c354a621d515262bcbb5f35e1a2dde8c828bdc7b9011cb5072&scene=21#wechat_redirect)  
  
  
  
  
  
