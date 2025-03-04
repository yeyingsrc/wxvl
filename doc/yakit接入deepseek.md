#  yakit接入deepseek   
原创 X1A0x1  无尽藏攻防实验室   2025-03-04 00:03  
  
   
  
2/64  
  
  
   
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/OKTibHnkK84yA83erVx1s3r6pckbia7wyux16qNavjsnZCnEuJoKyyYdiaZuQfmx4oZf6XP1jmsvLg9gRtYolZI6A/640?wx_fmt=png&from=appmsg "null")  
  
# 网络安全为人民  
  
师傅们好👋：本公众号现在已开启对常读和星标的公众号展示大图推送，为了不错过我们的网络安全干货，请星标🌟我们。这样，您就能快速掌握最新动态，与我们共同守护网络空间！感谢您的关注和支持！💖  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/OKTibHnkK84yA83erVx1s3r6pckbia7wyun1qzrrxZGmnRXgZc4l1xUFiaXBEgqxg05W0uO7PT4r0WG8u5fibG1bdw/640?wx_fmt=png&from=appmsg "")  
  
  
免责声明：  
  
无尽藏攻防实验室（本文公众号）的技术文章仅供学习参考，禁止用于其他！！！未经授权请勿利用文章中的技术资料对任何计算机系统进行入侵操作。由于传播、利用此文所提供的信息而造成的直接或间接后果和损失，均由使用者本人负责，无尽藏攻防实验室及作者不为此承担任何责任。如有侵权烦请告知,我们会立即删文章除并致歉，感谢您的理解和支持！  
  
使用前请遵守法律法规，使用本文章内容及POC等资源默认代表自愿遵守国家法律并由使用者本人承担一切法律后果。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/OKTibHnkK84yA83erVx1s3r6pckbia7wyuZCw2OVWY5X5ltH671MNxzmAayviaVEpzLcD3ZKILLia7aKU9yLQFy7eA/640?wx_fmt=gif&from=appmsg "")  
  
  
  
   
  
本文将介绍如何在yakit中接入deepseek  
### 配置  
  
进入yakit首页按照图片步骤点击  
  
![](https://mmbiz.qpic.cn/mmbiz_png/OKTibHnkK84zkzRaNl2BrsIlBUciacdMcmKaGD5bgSZEfwlalBlQYZdGCJFGNNKhCaslaF4GVKwFicEIOYc1y1fPA/640?wx_fmt=png&from=appmsg "null")  
  
  
  
全局配置中找到第三方应用配置  
  
![](https://mmbiz.qpic.cn/mmbiz_png/OKTibHnkK84zkzRaNl2BrsIlBUciacdMcmUzVcHSEO5icuUBzXZnhf43LImqEmibTKgapSJGQA89B8gpCK5LqpI3PQ/640?wx_fmt=png&from=appmsg "null")  
  
  
  
注意要将openai移到第一位  
  
点击openai进行配置，模型我们选择deepseek-ai/DeepSeek-V3，其他按照要求依次填入。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/OKTibHnkK84zkzRaNl2BrsIlBUciacdMcm6OPtJSleAicgDHbL6QjdxeLOoL90iamPgXfn5vFQROpl1icicCSic1juhxw/640?wx_fmt=png&from=appmsg "null")  
  
  
  
由于deepseek官方api常出现无法响应的情况，这边我们采用硅基来进行加速。### 验证  
  
完成配置后保存，我们打开yak run来进行测试  
  
输入以下语法就可以进行对话了  
```
msg = ai.Chat("你是谁？")~println(msg)
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/OKTibHnkK84zkzRaNl2BrsIlBUciacdMcm87v1fgpf3TZMvaKGreNOP5tXvbVDLzkXWibr9DTd65DKoGMibpOWnXmQ/640?wx_fmt=png&from=appmsg "null")  
  
  
  
   
  
   
  
   
  
  
