#  【高危漏洞】MongoDB Shell可能容易通过自动补全进行控制字符注入   
 皓月当空w   2025-03-04 01:20  
  
![](https://mmbiz.qpic.cn/mmbiz_png/h1AzajLJTBu7YOczrAwxaw0KAqv7gHNdsWu0BEtibKibaegocwoGb75HNNUpZ0ukoIu7XxnCpsONILQhseSns4zg/640?wx_fmt=png "")  
  
皓月当空，明镜高悬  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/h1AzajLJTBu7YOczrAwxaw0KAqv7gHNdZfzFIibGpEEEjcB4BuCfTfsf4KgL65xJd1EO5ibicom3eT9QDCHzvMr7w/640?wx_fmt=gif "")  
  
  
  
  
****  
**文末可体验bugSearch系统哦~**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/h1AzajLJTBu7YOczrAwxaw0KAqv7gHNdjgbENMv47awmAOrpibcD0W1Cyicz2b2RY2qRfeCSL29wSIMCPQMS3T8w/640?wx_fmt=png "")  
  
   
  
**漏洞名称**  
：  
MongoDB Shell可能容易通过自动补全进行控制字符注入  
  
**漏洞出现时间**  
：2025年2月27日  
  
**影响等级**  
：高危  
  
**漏洞说明：**  
  
   MongoDB Shell可能容易受到控制字符注入的影响，在这种情况下，控制mongosh自动补全功能的攻击者可以使用自动补全功能输入和运行混淆的恶意文本。这需要用户以用户使用“tab”自动完成文本的形式进行交互，该文本是攻击者准备的自动完成的前缀。此问题影响2.3.9之前的mongosh版本。只有当mongosh连接到部分或完全由攻击者控制的集群时，才能利用此漏洞。  
  
**影响版本：**  
- #### 2.3.9  
  
**修复方式：**  
- https://jira.mongodb.org/browse/MONGOSH-2024  
  
**相关链接：**  
- https://nvd.nist.gov/vuln/detail/CVE-2025-1691  
  
  
**最快的威胁情报，最全的漏洞评估**  
  
****  
  
  
  
  
  
  
Tips  
  
![](https://mmbiz.qpic.cn/mmbiz_png/h1AzajLJTBu7YOczrAwxaw0KAqv7gHNdjM6hZmEJpn7tvGpPUaMaWjmktwXWhnoEtcDFjczcwLC3v5tYxJV0JA/640?wx_fmt=png "")  
  
  
MongoDB  
是一个基于分布式文件存储的数据库。由C++语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。MongoDB  
是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/h1AzajLJTBsGR5jHw9fpxuTmXiaCdhv2XyzlwsZDUwVYeShmG5PSjqqOpUW3KCwb8q4pVmBso9BrqVTibFm576rQ/640?wx_fmt=png "")  
  
近期文章：  
  
[](http://mp.weixin.qq.com/s?__biz=Mzg4MDg5NzAxMQ==&mid=2247484843&idx=1&sn=772b303b7bfebde91f5178944bbcd375&chksm=cf6f7b37f818f22196b97caa7967c4f5d3f097608df9207c8860b0fc300e22378d7a211403d8&scene=21#wechat_redirect)  
  
[](http://mp.weixin.qq.com/s?__biz=Mzg4MDg5NzAxMQ==&mid=2247484823&idx=1&sn=33b81fbc4fb970c623e77d7a5ef46525&chksm=cf6f7b0bf818f21d3450c49112319228ee7fb2557f4914af6808ec6fc7fa3f9c5bd38780228c&scene=21#wechat_redirect)  
[【严重漏洞】 Apache FreeRDP 出现多个cve漏洞](http://mp.weixin.qq.com/s?__biz=Mzg4MDg5NzAxMQ==&mid=2247484859&idx=1&sn=eb3e9f6d87304e78741397da0c29936b&chksm=cf6f7b27f818f2313bb5832d63d54d85dc146bf3ebcc06278a2c8dd59605e7b1d442b0a3fa25&scene=21#wechat_redirect)  
  
  
[【高危漏洞】【未修复】EduSoho企培开源版存在未授权访问漏洞](http://mp.weixin.qq.com/s?__biz=Mzg4MDg5NzAxMQ==&mid=2247484863&idx=1&sn=fd7d16aef7c10a2699fb491b0ea02bbe&chksm=cf6f7b23f818f235e455e36c1ff8cf56d2dd964ea7f37a11ce67e7c63d2cb705f212ad02278e&scene=21#wechat_redirect)  
  
  
[【高危漏洞】达梦企业管理器（DEM）存在未授权访问漏洞](http://mp.weixin.qq.com/s?__biz=Mzg4MDg5NzAxMQ==&mid=2247484888&idx=1&sn=9e4d6603a8c3dc5d63532fa5571f1ccf&chksm=cf6f7b44f818f25269d6a7ebfa54a19b4f84d83e1c19fd110710a353d53bc0a67065685847fa&scene=21#wechat_redirect)  
  
  
[【高危漏洞】达梦大数据分析平台存在未授权访问漏洞](http://mp.weixin.qq.com/s?__biz=Mzg4MDg5NzAxMQ==&mid=2247484863&idx=2&sn=f6032b4c7a109a838b9392ef2950abce&chksm=cf6f7b23f818f23587ab5d2095882f8ff6a1bb0e205a67c49016dfe860ed4abe67b47e5467ac&scene=21#wechat_redirect)  
  
  
[【高危漏洞】北京启明星辰信息安全技术有限公司天镜Web应用检测系统存在弱口令漏洞](http://mp.weixin.qq.com/s?__biz=Mzg4MDg5NzAxMQ==&mid=2247484888&idx=2&sn=7fa6fe96332176f5c1feb21c9064a9e5&chksm=cf6f7b44f818f2523bf768e1a85a1edbbc6e5db8747a7c9982bf89e62e9915309b6e92fe3834&scene=21#wechat_redirect)  
  
  
[【严重漏洞】CVE-2023-34968  Samba信息泄露漏洞](http://mp.weixin.qq.com/s?__biz=Mzg4MDg5NzAxMQ==&mid=2247484853&idx=1&sn=e1e42f0123d773143f6eb8b56669ec70&chksm=cf6f7b29f818f23fe32ad614b7c99821d28a1f20e3b8ca2fc4543b754e6c1de14e878862b4ff&scene=21#wechat_redirect)  
  
  
[【高危漏洞】启明星辰信息安全技术有限公司4A统一安全管控平台存在命令执行漏洞](http://mp.weixin.qq.com/s?__biz=Mzg4MDg5NzAxMQ==&mid=2247484849&idx=1&sn=30d011f2463c4912c42491289a590062&chksm=cf6f7b2df818f23b8376cbb2f6e2da22b223ea184c98106ddbbae747b44a0d2299a3c3202d27&scene=21#wechat_redirect)  
  
  
[【高危漏洞】CVE-2023-40195-Apache Airflow 存在反序列化漏洞](http://mp.weixin.qq.com/s?__biz=Mzg4MDg5NzAxMQ==&mid=2247484843&idx=2&sn=b6bcc3857a123bc3a0f79238617ada50&chksm=cf6f7b37f818f2212881d7cbb330d01874949be7a51e6433fbc79450d7ec968bc3eaaca37b51&scene=21#wechat_redirect)  
  
  
  
  
  
  
  
BugSearch正式于2023年12月13日开始内测，功能仅开放每日漏洞更新。  
  
- 2023年10月1日 开始筹备  
  
- 2023年11月10日 漏洞检测模块完成  
  
- 2023年11月25日 前端UI完成  
  
- 2023年12月5日 前端UI替换优化  
  
- 2023年12月13日 正式开始内测  
  
- 2023年12月18日 完成高级搜索功能  
  
- 2023年12月18日 站点人数统计  
  
- 2024年1月9日 漏洞点击统计功能  
  
- 2024年1月10日 UI调整  
  
- 2024年4月29日 新增放假啦功能  
  
- 2024年5月7日 更新UI，适配手机端  
  
- 2024年5月7日 更新获取方式，现在公众号回复token，直接返回链接，访问后即可登录  
  
- 2024年5月17日 新增更多咨询，以及freebuff，安全客文章  
  
- 2024年5月18日 新增先知文章  
  
- 2024年5月22日 开通机器人功能  
  
- 2024年5月23日 新增数据源  
  
- 2024年6月5日 新增机器人推送早报功能  
  
- 2025年3月4日 项目重新启动  
  
高级搜索已完成。  
  
  
使用方式：  
  
  
**首先需要关注公众号，然后发送token，直接访问概链接就可以登陆了。**  
  
  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/h1AzajLJTBsiaHmnZSibt1icLpa0yYG4GKBxZ0L3hPUQYcLEGqjy2lV24GCnZxRV4iaas9iaqac7FC1HKjWL7zvsicyg/640?wx_fmt=png&from=appmsg "")  
  
  
  
**仪表盘页面：**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/h1AzajLJTBsiaHmnZSibt1icLpa0yYG4GKB0GqgZwaGsjXekYhPqLzibjgdfgO01p2UrC2vBa2VjImTicib6Xjy7SibUg/640?wx_fmt=png&from=appmsg "")  
  
  
机器人功能暂停ing，有需要可私聊联系  
  
  
  
