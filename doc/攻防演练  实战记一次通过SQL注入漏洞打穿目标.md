#  攻防演练 | 实战记一次通过SQL注入漏洞打穿目标   
原创 zkaq-执念  掌控安全EDU   2025-03-03 04:02  
  
扫码领资料  
  
获网安教程  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrpvQG1VKMy1AQ1oVvUSeZYhLRYCeiaa3KSFkibg5xRjLlkwfIe7loMVfGuINInDQTVa4BibicW0iaTsKw/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/b96CibCt70iaaJcib7FH02wTKvoHALAMw4fchVnBLMw4kTQ7B9oUy0RGfiacu34QEZgDpfia0sVmWrHcDZCV1Na5wDQ/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
  
# 本文由掌控安全学院 -   执念 投稿  
  
**来****Track安全社区投稿~**  
  
**千元稿费！还有保底奖励~（https://bbs.zkaq.cn）**  
  
**免责声明**：本报告仅用于安全研究和授权测试，未经授权的渗透测试行为是违法的。请确保在合法授权的情况下进行安全测试。  
## 前言  
  
本次攻防演练的目标单位是某交通运输企业及其子公司，旨在评估其网络系统的安全性。通过信息搜集、漏洞利用、内网渗透等步骤，最终成功获取了目标系统的控制权限，并发现了多个安全漏洞。以下是详细的渗透测试过程：  
## 信息搜集  
  
第一步，通过爱企查平台分析了目标企业的基本信息及其子公司情况，确认了企业的域名和主要业务系统。爱企查提供了企业的注册信息、股东结构、子公司列表等详细信息，帮助我们快速了解目标企业的组织架构和业务范围。  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQY0yVrvaibtlwjQgVOBrFLMzkMlI9TiaqoSVXzJHIqWAd7ibBoAicU18MoPA/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
接着利用网络空间测绘平台拓展攻击面，我比较喜欢利用鹰图平台（网络空间测绘平台）对目标企业进行资产扫描，快速检索出企业的业务系统及其子公司的相关资产。鹰图平台提供了丰富的资产信息，包括域名、IP地址、开放端口、服务类型等，帮助我们全面了解目标企业的网络架构。尤其是通过ICP备案信息反查功能，发现了多个子公司的业务系统。  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYmj7bWiaXPHicMAKF8MJxD4oFMcL8l9ibunNkNyg8QohnfKicAwL2pzNiapg/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
此外，我们还使用了子域名枚举工具（如Sublist3r）对目标企业的域名进行了子域名枚举。通过枚举，发现了多个子域名，这些子域名可能对应着不同的业务系统和服务。我们对这些子域名进行了初步的访问和测试，以寻找潜在的漏洞。  
## SQL注入漏洞  
  
通过信息搜集发现，该公司有很多的子公司，因此在多次尝试母公司网站渗透无果后，我们把目标锁定到子公司了。在其中一个子公司资产下面找到一个新闻发布系统，里面点击带有交互的功能菜单按钮，发现一处sql注入漏洞，注入数据包如下图所示，注入参数为riqi参数  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQY4AibqaW8ibCXByZic6NibO59FGFPmS3ZZSnvZZrPjdySiaszM827XU1LeIw/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
接着利用sqlmap工具成功获取了数据库的Banner信息，确认数据库版本为Sqlserver 2012，获得了当前数据库为web。这一步骤帮助我们了解了目标系统的数据库类型和版本，为后续的漏洞利用提供了重要信息。  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYHZ8bcIbxSicI9dYspUPciaehm6Q8qCErUorzPfVRNdbJODriaqdQN10gg/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
通过注入获取了管理员账号的MD5加密密码，我使用了在线MD5解密工具和彩虹表进行解密，最终成功获取了明文密码XXX  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYYZLquZ2tAdd94GicLs7micicn3X6giaSJYreTHBR0aQCxHh7tO2Y9Qu5zQ/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
有意思的是，网站的最下方给出了管理后台的链接，这就免得我们使用御剑去爆破后台路径了。使用上面获取的管理员账号和密码成功登录了新闻发布系统的后台管理界面。在后台中，我们发现了一些敏感信息和功能模块，为进一步的渗透提供了便利。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYvymgJr6xM6FB6w9NmrEiajE07MCkI2M9GtZXM5IBY4NiccfVYo4JaKew/640?wx_fmt=png&from=appmsg "")  
  
img  
  
当然要想扩大战果，最理想的就是找到文件上传点然后getshell。老天爷似乎也很照顾我哈哈，在管理员后台中，发现了一个文件上传功能，存在任意文件上传漏洞。  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQY3r7kKxxWeJce1IZibp8oXCGk0s9zv2WXu9eOhNicgj3sgbx2OickP9PmQ/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYCH9XotGR3Vq1SXicfE46Jks5tXkh7onIsgy9Ok3R4fxL0k7bXCKLWXA/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
上传了一个asp脚本的 WebShell文件，成功获取了服务器的控制权限。我使用了流量数据加密的WebShell工具—哥斯拉进行连接，并成功执行了系统命令。  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYvaqgJlQuPCib4UOibdHWOtic9YrD061PaQib8SY5aDyEdJL5djW6xAC8TA/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
为了进一步控制目标服务器，又向目标上传并通过静默安装的方式安装todesk远程管理工具，从而间接获取目标服务器的远程管理权限。(静默安装的方式参见https://www.todesk.com/helpcenter/solo-65.html)  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQY1W9fibtHCVYLwRVH0yzoZMQ9q7AboZkBsicVVrf19WicHr4HKnywPkYrg/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQY1bAR4zpVicoWhb1rvgyOOo8rYUaaNyXd4v4fHNqXecfC4vFibTaKXggQ/640?wx_fmt=png&from=appmsg "")  
  
img  
  
但是登录桌面后发现目标已经安装了向日葵远程管理软件，为了隐蔽操作，故将todesk进行卸载，使用目标的向日葵进行远程操作。这一步骤帮助我们避免了被目标系统管理员发现的风险。  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYLOnaKaIAAjCxl5Uqf2iacxicA1QKZUFWbUwlSfKCicDekr25dc8XGy08w/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYXGpNEq5Hliaz9XMIKVgBWad6SRI1ZBtxicyh4ibvhHm2I9e32RiccXXTLg/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
考虑到后续的内网横向渗透，我通过上传的WebShell又上线了Cobalt Strike (CS) 木马。尝试了多种提权技术（如DLL注入、服务提权等）来提升权限，最后成功获取了系统的最高权限。  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYUbOhicTibSpyR0RGmrVGkOnw13ZOrk5DyaTiaTrWKt5aHFMsvIuFKWVBg/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
利用mimikatz插件也成功地获取了管理员明文密码，下一步就是进行内网横向移动扩大战果了。  
## 内网渗透  
  
可能是运气比较好，这里通过上面获取的明文密码进行口令碰撞攻击，直接拿下一台内网websphere服务器控制权限  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYNwkCVs1tDb9picbDl7GqSkvZQ1vx1RXKFRjLicFCLHsG1raATer0jV4w/640?wx_fmt=png&from=appmsg "")  
  
img  
  
可以发现，这台websphere服务器集中管理平台管理内网7台服务器节点  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQY8J3uiage8E5weDqp4lrfI9KXKGQXicOvDyvSibks3zVrPvqI4yJxuCwAw/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
使用fscan对192.168.0.0/16网段进行IP扫描探测，发现大量SSH弱口令漏洞  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYAKiaQ7PXu8GHrFa7EwQP9tZYwMlHebkOvzaqm7PG6Uia6ia3j9JIcEqkg/640?wx_fmt=png&from=appmsg "")  
  
img  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQY0889TL4o3Nicic9KDkBYHztQAoB3MJSP1T2MExme5oXib5TtTLqjS7yyw/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
fscan扫描中发现一台开放1433端口的mssql服务器存在弱口令sa/sa  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYp0XOQsPdKjibbriaYzp05g2oTfPvTYuVwo1B0BkwFvQD7EHIxdcWicaLw/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
利用MDUT工具进行xp_cmdshell提权，添加管理员账户，开启远程桌面3389服务，最终成功登陆服务器：  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_jpg/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYd051fQEAvtfnMF2icsW1aIo8yKYT4z0V0KKt8YQRHqm2EOwcXAy2ia0A/640?wx_fmt=jpeg&from=appmsg "null")  
  
img  
## 摸入核心业务系统  
  
在上面主机上进行信息搜集，通过查看google浏览器保存的密码，获取大量核心业务系统的访问平权限  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQY5ZiabOqnUpWic5MsTdbW0LKkib6vP0IrWHKOPjEuNppV28XTLkDviar3MA/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
系统1：卫星定位服务平台，该系统集成了卫星定位、通信、数据处理，包括交通管理、车辆定位、车辆监控、智能出行、报表分析等功能  
  
![img](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQY1nSWQUNpU7I8mCrorzVudyaXhMFLYoeiaFgCjasqep8MJDaicP9EpBAQ/640?wx_fmt=png&from=appmsg "null")  
  
img  
  
系统2：视频监控管理系统:，该系统可以实时查看所有下属公交车的监控视频画面  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrDvwfYpUeo6mym3GykwrQYrv6I3UnmtpeCK4ku8llwt0bicmx3a6gvbbyPictiam0BYiap44ASVdSkCQ/640?wx_fmt=png&from=appmsg "")  
  
img  
  
还有一些工业控制方面的系统，因为较为敏感（这些系统可能控制着关键的基础设施，一旦被攻击，可能会造成严重的后果），这里就不放出来啦，总之是分数拉满了，目标出局了。打完  
~收工  
## 总结  
  
本次演练通过对目标子公司多个网站尝试外网突破，最终通过新闻发布网站SQL注入漏洞成功打入内网，之后通过对内网的信息搜集、横向移动的方式获取到多台内网终端系统权限。演练中使用的手段包括常规WEB攻击、操作系统攻击、数据库攻击等，通过WEBSHELL、todesk等远控软件的安装建立隐蔽的安全的回传隧道，最终效果可以实现信息欺骗、链路阻断、节点至瘫、接管控制等攻击效果。对于相关企业的安全建议：  
  
  
1.**加强SQL注入防护**：对输入参数进行严格的过滤和验证，使用预编译语句或ORM框架来防止SQL注入。  
  
2.**修复文件上传漏洞**：限制上传文件的类型和大小，并对上传文件进行严格的检查和过滤。  
  
3.**强化密码策略**：避免使用弱口令，定期更换密码，并启用多因素认证。  
  
4.**内网安全加固**：对内网系统进行定期的安全审计，及时发现并修复漏洞。  
  
5.**监控与响应**：部署安全监控系统，及时发现异常行为并进行响应。  
  
6.**员工安全意识培训**：定期对员工进行安全意识培训，防止社会工程学攻击。  
  
7.**网络隔离**：对关键系统进行网络隔离，限制内网访问权限，减少攻击面。  
  
申明：本公众号所分享内容仅用于网络安全技术讨论，切勿用于违法途径，  
  
所有渗透都需获取授权，违者后果自行承担，与本号及作者无关，请谨记守法.  
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/BwqHlJ29vcqJvF3Qicdr3GR5xnNYic4wHWaCD3pqD9SSJ3YMhuahjm3anU6mlEJaepA8qOwm3C4GVIETQZT6uHGQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1&tp=webp "")  
  
**没看够~？欢迎关注！**  
  
  
  
**分享本文到朋友圈，可以凭截图找老师领取**  
  
上千**教程+工具+交流群+靶场账号**哦  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrpvQG1VKMy1AQ1oVvUSeZYhLRYCeiaa3KSFkibg5xRjLlkwfIe7loMVfGuINInDQTVa4BibicW0iaTsKw/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
******分享后扫码加我！**  
  
  
**回顾往期内容**  
  
[零基础学黑客，该怎么学？](http://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247487576&idx=1&sn=3852f2221f6d1a492b94939f5f398034&chksm=fa686929cd1fe03fcb6d14a5a9d86c2ed750b3617bd55ad73134bd6d1397cc3ccf4a1b822bd4&scene=21#wechat_redirect)  
  
  
[网络安全人员必考的几本证书！](http://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247520349&idx=1&sn=41b1bcd357e4178ba478e164ae531626&chksm=fa6be92ccd1c603af2d9100348600db5ed5a2284e82fd2b370e00b1138731b3cac5f83a3a542&scene=21#wechat_redirect)  
  
  
[文库｜内网神器cs4.0使用说明书](http://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247519540&idx=1&sn=e8246a12895a32b4fc2909a0874faac2&chksm=fa6bf445cd1c7d53a207200289fe15a8518cd1eb0cc18535222ea01ac51c3e22706f63f20251&scene=21#wechat_redirect)  
  
  
[记某地级市护网的攻防演练行动](https://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247543747&idx=1&sn=c7745ecb8b33401ae317c295bed41cc8&token=74838194&lang=zh_CN&scene=21#wechat_redirect)  
  
  
[](https://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247542576&idx=1&sn=d9f419d7a632390d52591ec0a5f4ba01&scene=21#wechat_redirect)  
[手把手教你CNVD漏洞挖掘 + 资产收集](https://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247542576&idx=1&sn=d9f419d7a632390d52591ec0a5f4ba01&token=74838194&lang=zh_CN&scene=21#wechat_redirect)  
  
  
[【精选】SRC快速入门+上分小秘籍+实战指南](http://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247512593&idx=1&sn=24c8e51745added4f81aa1e337fc8a1a&chksm=fa6bcb60cd1c4276d9d21ebaa7cb4c0c8c562e54fe8742c87e62343c00a1283c9eb3ea1c67dc&scene=21#wechat_redirect)  
  
##     代理池工具撰写 | 只有无尽的跳转，没有封禁的IP！  
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/BwqHlJ29vcqJvF3Qicdr3GR5xnNYic4wHWaCD3pqD9SSJ3YMhuahjm3anU6mlEJaepA8qOwm3C4GVIETQZT6uHGQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1&tp=webp "")  
  
点赞+在看支持一下吧~感谢看官老爷~   
  
你的点赞是我更新的动力  
  
  
