#  Ollama大模型工具安全吗？深度解析潜在漏洞与防护指南   
原创 道玄安全  道玄网安驿站   2025-03-04 07:28  
  
**“**  
 AI工具的安全性，决定了技术落地的边界**”**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/L369x9IF3yPA9bic9zzTydWv4XTTHH2NAiamMp8Kxsh4s2lukPuyuwnia3NiaHkiaU8a3JGFhLvNnYvtLvHTFAd91Rw/640?wx_fmt=png&from=appmsg "")  
  
      
看到了，**关注一下**  
不吃亏啊，点个赞转发一下啦，WP看不下去的，可以B站搜：**标松君**  
，UP主录的打靶视频，欢迎关注。顺便宣传一下星球：**重生者安全，**  
 里面每天会不定期更新**OSCP**  
知识点，**车联网**  
，**渗透红队**  
以及**漏洞挖掘工具**  
等信息分享，欢迎加入；以及想挖**SRC逻辑漏洞**  
的朋友，可以私聊。  
  
  
  
  
  
01  
  
—  
  
  
### Ollama的架构风险：为何漏洞容易被忽视？  
  
  
Ollama作为一款开源的本地大模型部署工具，因其轻量化和易用性迅速走红。然而，随着用户量激增，其安全性问题逐渐浮出水面。本文从漏洞风险、真实案例到防护方案，为你全面解析Ollama的安全现状  
  
  
**一、容器化依赖的“双刃剑”**  
1. Ollama默认依赖Docker容器部署模型，但早期版本的容器权限配置宽松（如未启用--user  
参数），可能导致**容器逃逸**  
攻击（攻击者从容器内突破到宿机）。  
  
  
2.API接口的“开放之痛”  
- **模型劫持**  
：攻击者通过API注入恶意指令。  
  
- **数据泄露**  
：窃取本地部署的私有模型或对话记录。  
  
- 默认端口11434  
未强制身份验证，若暴露在公网或内网不可信环境，可能导致：  
  
### 二、已知高危漏洞清单  
<table><thead><tr><th style="color: rgb(var(--ds-rgb-label-1));padding-left: 0px;border-bottom: 1px solid rgb(var(--ds-rgb-label-3));border-top: 1px solid rgb(var(--ds-rgb-label-3));font-weight: 600;text-align: left;"><strong><span leaf="">漏洞类型</span></strong></th><th style="color: rgb(var(--ds-rgb-label-1));border-bottom: 1px solid rgb(var(--ds-rgb-label-3));border-top: 1px solid rgb(var(--ds-rgb-label-3));font-weight: 600;text-align: left;"><strong><span leaf="">影响版本</span></strong></th><th style="color: rgb(var(--ds-rgb-label-1));border-bottom: 1px solid rgb(var(--ds-rgb-label-3));border-top: 1px solid rgb(var(--ds-rgb-label-3));font-weight: 600;text-align: left;"><strong><span leaf="">风险等级</span></strong></th><th style="color: rgb(var(--ds-rgb-label-1));border-bottom: 1px solid rgb(var(--ds-rgb-label-3));border-top: 1px solid rgb(var(--ds-rgb-label-3));font-weight: 600;text-align: left;"><strong><span leaf="">修复方案</span></strong></th></tr></thead><tbody><tr><td style="padding-left: 0px;border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">容器提权漏洞</span></section></td><td style="border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">&lt;0.1.28</span></section></td><td style="border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">高危</span></section></td><td style="border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">升级至0.1.33+，限制容器权限</span></section></td></tr><tr><td style="padding-left: 0px;border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">API未授权访问</span></section></td><td style="border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">所有默认配置</span></section></td><td style="border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">中高危</span></section></td><td style="border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">配置身份验证或防火墙规则</span></section></td></tr><tr><td style="padding-left: 0px;border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">模型文件注入风险</span></section></td><td style="border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">所有版本</span></section></td><td style="border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">中危</span></section></td><td style="border-bottom: 1px solid rgb(var(--ds-rgb-label-3));"><section><span leaf="">仅加载可信来源模型</span></section></td></tr></tbody></table>  
### 三、用户必做的4项安全防护  
###   
  
**1.升级**  
1.   命令行执行 ollama update  
，确保版本≥0.1.33。  
  
1. **禁用自动下载**  
：通过环境变量OLLAMA_HOST  
限制模型拉取源。  
  
**2.锁死API端口******  
1. **修改默认端口**  
：在启动时指定OLLAMA_HOST=0.0.0.0:自定义端口  
。  
  
1. **启用基础认证**  
（示例代码）：  
```
# 生成密码文件  
echo "user:$(openssl passwd -5 密码)" > ollama_auth  
# 启动时加载认证  
OLLAMA_AUTH=ollama_auth ollama serve  
```  
  
****  
1.   
1.   
1.   
1.   
**3.最小权限原则**  
1.   永远不要以root  
身份运行Ollama！建议创建专用低权限用户。  
  
1.   Docker用户需添加--user 1000:1000  
参数限制容器权限。  
  
**4.模型来源可信化**  
1.   避免从非官方渠道下载.bin  
模型文件，警惕“魔改模型”植入后门代码。  
  
  
四、未来威胁：AI工具链的安全盲区  
  
  
**1.供应链攻击**  
：第三方模型库可能成为投毒目标（参考PyTorch的依赖劫持事件）。  
  
2.提示词注入  
：攻击者通过精心构造的输入绕过模型安全限制。  
  
**3.开发团队响应**  
：Ollama已启动漏洞赏金计划，但社区协作仍需加强  
  
  
  
  
02  
  
—  
安全不是功能，而是底线  
  
Ollama的便捷性让AI触手可及，但若忽视安全配置，可能让本地部署的模型成为攻击者的“跳板”。立即检查你的Ollama环境，转发本文提醒团队开发者——防范0Day，从此刻开始。  
  
  
**#互动话题**  
  
你在使用Ollama时遇到过安全警告吗？欢迎留言分享你的防护经验！  
  
  
  
  
  
  
免责声明：  
### 本人所有文章均为技术分享，均用于防御为目的的记录，所有操作均在实验环境下进行，请勿用于其他用途，否则后果自负。  
  
第二十七条：任何个人和组织不得从事非法侵入他人网络、干扰他人网络正常功能、窃取网络数据等危害网络安全的活动；不得提供专门用于从事侵入网络、干扰网络正常功能及防护措施、窃取网络数据等危害网络安全活动的程序和工具；明知他人从事危害网络安全的活动，不得为其提供技术支持、广告推广、支付结算等帮助  
  
第十二条：  国家保护公民、法人和其他组织依法使用网络的权利，促进网络接入普及，提升网络服务水平，为社会提供安全、便利的网络服务，保障网络信息依法有序自由流动。  
  
任何个人和组织使用网络应当遵守宪法法律，遵守公共秩序，尊重社会公德，不得危害网络安全，不得利用网络从事危害国家安全、荣誉和利益，煽动颠覆国家政权、推翻社会主义制度，煽动分裂国家、破坏国家统一，宣扬恐怖主义、极端主义，宣扬民族仇恨、民族歧视，传播暴力、淫秽色情信息，编造、传播虚假信息扰乱经济秩序和社会秩序，以及侵害他人名誉、隐私、知识产权和其他合法权益等活动。  
  
第十三条：  国家支持研究开发有利于未成年人健康成长的网络产品和服务，依法惩治利用网络从事危害未成年人身心健康的活动，为未成年人提供安全、健康的网络环境。  
  
  
  
  
  
  
