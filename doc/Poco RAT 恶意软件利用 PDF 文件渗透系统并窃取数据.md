#  Poco RAT 恶意软件利用 PDF 文件渗透系统并窃取数据   
邑安科技  邑安全   2025-03-03 02:48  
  
更多全球网络安全资讯尽在邑安全  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1N39PtINn8suTP79ZeFVvrsxlFH0ichKmc8tZtK8e3yaG7PPagJrHE2MqFg0IqmU0YkadibX7TnHxibEAp0lxLicLg/640?wx_fmt=png&from=appmsg "")  
  
Poco RAT 恶意软件的新变体已成为对拉丁美洲讲西班牙语的组织构成的重大威胁，它利用复杂的 PDF 诱饵和基于云的交付系统来渗透网络并泄露敏感数据。  
  
该活动与网络雇佣兵组织 Dark Caracal 相关联，代表了以前与 Bandook 远程访问木马相关的策略的演变，现在适用于更广泛的网络钓鱼作和金融间谍活动。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1N39PtINn8suTP79ZeFVvrsxlFH0ichKmTib2pAVibwR28KhFcDfd5ajfa3SGJlVqQMQOjicbNE1ljjYuvBHeicNK0Q/640?wx_fmt=png&from=appmsg "")  
  
Dark Caracal 攻击链  
##   
## 攻击链  
  
该活动从伪装成财务通知的网络钓鱼电子邮件开始，通常引用未付发票或税务文件。  
  
攻击者附加模仿合法组织的 PDF 文件，包括 BBVA Provincial 等委内瑞拉银行和 Global Supply Services 等工业公司。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1N39PtINn8suTP79ZeFVvrsxlFH0ichKmGNUTcg5UhKUHybdhPvS4xB1q39b9Xb6gEwYPvYpz0ibqNRXTSTcFvGw/640?wx_fmt=png&from=appmsg "")  
  
带有 PDF 诱饵的网络钓鱼电子邮件  
  
这些诱饵使用模糊的图形和元数据字段，其中填充了西班牙语作者姓名，如“Rene Perez”和“Keneddy Cedeño”，以显得真实，同时逃避初步检测。  
  
打开后，PDF 会将受害者重定向到在 Google Drive 和 Dropbox 等平台上托管恶意 .rev 档案的缩短 URL。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1N39PtINn8suTP79ZeFVvrsxlFH0ichKmOLDSSknkwPMP07IxksydwjdWtJ1Ks33Ubia1Asqia9hJ2yfxeGV6ANFg/640?wx_fmt=png&from=appmsg "")  
  
PDF 诱饵的防病毒扫描  
  
这种技术利用了对合法云服务的信任——在 2024-2025 年活动期间，只有 7% 的诱饵文档触发了防病毒警报。  
.rev 文件最初设计用于修复损坏的档案，现在用作 Poco RAT 的 dropper 的隐身工具，这是一种基于 Delphi 的可执行文件，通过直接注入 iexplore.exe 等进程来避免磁盘写入。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1N39PtINn8suTP79ZeFVvrsxlFH0ichKmYpZYuZO0XlYn6qBdrUSdAHuTGKZIiasGha1c6lnKEYEPFNxQoicG9VkQ/640?wx_fmt=png&from=appmsg "")  
  
冒充委内瑞拉公司 Zoom 的文件的诱饵文档  
##   
## 技术规避和扩大目标  
  
Dark Caracal 的最新工具采用多层混淆：  
- 动态 API 解析隐藏恶意函数调用  
  
- 使用每个构建密钥的 Twofish 加密保护嵌入的字符串  
  
- 异常处理程序劫持重定向代码执行以绕过调试器  
  
与之前的 Bandook 活动相比，该组织扩大了其行业目标，最近 49% 的攻击冒充了科技公司，比 2023 年增加了 33%。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1N39PtINn8suTP79ZeFVvrsxlFH0ichKmIbTFzS3PF68Es0bt2WB3KaoxRobUXic0xXh1gN0wNwhrK48ZDn5Qqyg/640?wx_fmt=png&from=appmsg "")  
  
Poco RAT dropper 中的控制转移到初始化功能  
  
金融机构 （10%） 和制造企业 （10%） 仍然是主要目标，这反映出人们对知识产权和交易记录的持续关注。  
## Poco RAT 的间谍工具包  
  
部署后，恶意软件会进行全面侦察：  
1. 环境分析：通过注册表检查 （SOFTWARE\Oracle\VirtualBox） 和端口扫描 （VMware 的 0×5658） 检测虚拟化  
  
1. 数据收集：使用“@&）”分隔符将用户名、作系统版本和 RAM 指标收集到结构化报告中  
  
1. C2 通信：通过向 IP （如 193.233.203.63）发送检测信号消息来保持持久性，同时在端口 6211—6543 之间循环以避免阻塞  
  
**命令执行能力包括：**  
- 屏幕截图 （T-05）  
  
- 无文件有效负载执行 （T-03）  
  
- 直通命令提示符访问 （T-06）  
  
## 与 Bandook 运营的基础设施链接  
  
Positive Technologies 的分析揭示了 Poco RAT 和 Dark Caracal 的传统工具之间重叠的基础设施（表 5）：  
- AS200019 （AlexHost SRL）：托管 Poco RAT （185.216.68.121） 和 Bandook C2s （185.216.68.143）  
  
- AS44477 （Stark Industries Ltd.）：由 Poco RAT （94.131.119.126） 和 Bandook 服务器从 2023 年共享。  
  
这种基础设施协同作用使恶意软件系列之间能够平稳过渡，Poco RAT 样本同比增长 36%（483 个对 355 个 Bandook 文件）。  
  
随着 Dark Caracal 不断完善其策略，此活动中社会工程和云滥用的混合凸显了将用户教育和技术控制相结合的深度防御策略的必要性。  
  
原文来自:   
cybersecuritynews.com  
  
原文链接: https://cybersecuritynews.com/poco-rat-malware-exploits-pdf-files/  
  
欢迎收藏并分享朋友圈，让五邑人网络更安全  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/1N39PtINn8tD9ic928O6vIrMg4fuib48e1TsRj9K9Cz7RZBD2jjVZcKm1N4QrZ4bwBKZic5crOdItOcdDicPd3yBSg/640?wx_fmt=jpeg "")  
  
欢迎扫描关注我们，及时了解最新安全动态、学习最潮流的安全姿势！  
  
推荐文章  
  
1  
  
[新永恒之蓝？微软SMBv3高危漏洞（CVE-2020-0796）分析复现](http://mp.weixin.qq.com/s?__biz=MzUyMzczNzUyNQ==&mid=2247488913&idx=1&sn=acbf595a4a80dcaba647c7a32fe5e06b&chksm=fa39554bcd4edc5dc90019f33746404ab7593dd9d90109b1076a4a73f2be0cb6fa90e8743b50&scene=21#wechat_redirect)  
  
  
2  
  
[重大漏洞预警：ubuntu最新版本存在本地提权漏洞（已有EXP）　](http://mp.weixin.qq.com/s?__biz=MzUyMzczNzUyNQ==&mid=2247483652&idx=1&sn=b2f2ec90db499e23cfa252e9ee743265&chksm=fa3941decd4ec8c83a268c3480c354a621d515262bcbb5f35e1a2dde8c828bdc7b9011cb5072&scene=21#wechat_redirect)  
  
  
  
  
  
