#  安卓手机通过Cellebrite的Linux USB零日漏洞解锁   
邑安科技  邑安全   2025-03-03 02:48  
  
更多全球网络安全资讯尽在邑安全  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1N39PtINn8suTP79ZeFVvrsxlFH0ichKmF9D2atxmcLAX9Gxq9dR48uRWm5Rfd31gNc0C5zKKl1HFKEn3QefEGw/640?wx_fmt=png&from=appmsg "")  
  
  
国际特赦组织的安全实验室在塞尔维亚揭露了一个复杂的网络间谍活动，当局使用 Cellebrite 开发的零日漏洞利用链解锁了一名学生活动家的 Android 手机。  
  
这次攻击发生在 2024 年 12 月 25 日，利用 Linux 内核 USB 驱动程序中的漏洞绕过三星 Galaxy A32 设备上的锁屏保护。  
  
取证分析显示，该漏洞利用链滥用传统的 USB 驱动程序怪癖来获得 root 访问权限，从而能够提取数据并尝试安装监控工具。  
  
这一事件凸显了针对公民社会的系统性滥用数字取证工具，并凸显了 Android 在防御物理访问攻击方面存在重大漏洞。  
##   
## 漏洞利用链目标、USB 驱动程序  
  
该攻击利用一系列复杂的模拟 USB 设备来触发 Linux 内核中的内存损坏漏洞。取证日志显示，当局通过 Cellebrite 的 Turbo Link 适配器连接了多个恶意外围设备，包括：  
- 利用 CVE-2024-53104 的 Chicony CNF7129 UVC 网络摄像头 （VID：0x04f2），这是 USB 视频类驱动程序的帧速率限制怪癖中的越界写入。  
  
- 利用 CVE-2024-53197 的 Creative Extigy SoundBlaster （VID：0x041e），允许在 ALSA 声卡初始化期间损坏描述符。  
  
- 一个 Anton 触摸板 （VID：0x1130） 利用 CVE-2024-50302 通过 HID 报告泄露未初始化的内核内存。  
  
这些漏洞已在 Linux 内核版本 6.6+ 和 2025 年 2 月的 Android 安全公告中修补，存在于 2010-2013 年的代码中。  
  
攻击者将它们组合在一起以实现权限提升，内核日志显示 root shell 访问在最终 USB HID 设备连接后 10 秒内就证明了这一点。  
  
受害者是一名被称为“Vedran”的 23 岁学生，在 2024 年 12 月反对塞尔维亚执政党的抗议活动中被便衣警察拘留。设备日志证实了他的描述：  
  
漏洞利用后活动包括使用 / 进行文件系统枚举，以及部署 Cellebrite 的“falcon”二进制文件以进行高级数据提取。虽然目标 APK 安装因生物识别锁定而失败，但数据泄露暴露了通话记录、消息和抗议协调详细信息。findgrep  
  
Google 的威胁分析小组与 Amnesty 合作分析了这些漏洞，从而为三个 CVE 提供了补丁。但是，由于供应商更新周期分散，截至 2025 年 3 月，超过 40% 的 Android 设备仍未打补丁1。Cellebrite 于 2025 年 2 月 25 日暂停了塞尔维亚客户，并表示：  
  
“我们认为停止相关客户使用我们的产品是合适的......我们的合规计划确保合乎道德、合法的使用。  
  
批评者认为该措施缺乏透明度，因为 Cellebrite 拒绝披露其暂停的持续时间或恢复的人权保障措施。尽管自 2022 年以来在 12 个州记录了滥用行为，但该公司的 Premium UFED 工具包仍在 78 个国家/地区运行。  
  
原文来自:   
cybersecuritynews.com  
  
原文链接: https://cybersecuritynews.com/android-phone-cellebrites-usb-zero-day/  
  
欢迎收藏并分享朋友圈，让五邑人网络更安全  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/1N39PtINn8tD9ic928O6vIrMg4fuib48e1TsRj9K9Cz7RZBD2jjVZcKm1N4QrZ4bwBKZic5crOdItOcdDicPd3yBSg/640?wx_fmt=jpeg "")  
  
欢迎扫描关注我们，及时了解最新安全动态、学习最潮流的安全姿势！  
  
推荐文章  
  
1  
  
[新永恒之蓝？微软SMBv3高危漏洞（CVE-2020-0796）分析复现](http://mp.weixin.qq.com/s?__biz=MzUyMzczNzUyNQ==&mid=2247488913&idx=1&sn=acbf595a4a80dcaba647c7a32fe5e06b&chksm=fa39554bcd4edc5dc90019f33746404ab7593dd9d90109b1076a4a73f2be0cb6fa90e8743b50&scene=21#wechat_redirect)  
  
  
2  
  
[重大漏洞预警：ubuntu最新版本存在本地提权漏洞（已有EXP）　](http://mp.weixin.qq.com/s?__biz=MzUyMzczNzUyNQ==&mid=2247483652&idx=1&sn=b2f2ec90db499e23cfa252e9ee743265&chksm=fa3941decd4ec8c83a268c3480c354a621d515262bcbb5f35e1a2dde8c828bdc7b9011cb5072&scene=21#wechat_redirect)  
  
  
  
  
  
