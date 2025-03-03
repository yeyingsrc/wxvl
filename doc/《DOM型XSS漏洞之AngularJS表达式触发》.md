#  《DOM型XSS漏洞之AngularJS表达式触发》   
原创 柠檬赏金  柠檬赏金猎人   2025-03-03 10:13  
  
   
  
### 介绍  
  
AngularJS 是一个 JavaScript 框架。它可通过script标签添加到 HTML 页面。AngularJS 通过 指令 扩展了 HTML，且通过 表达式 绑定数据到 HTML。ng-app作用是告诉子元素以下的指令是归angularJs的，angularJs会识别，这种技术在目标对尖括号进行过滤时非常有用。  
### 目标  
  
执行跨站点脚本攻击，执行 AngularJS 表达式并调用该 alert  
 函数。  
### 步骤  
1. 1. 测试目标。  
  
```
https://0a7f008104730a4580e171d000f700d6.web-security-academy.net/
```  
1. 2. 在搜索框中输入随机的字母字符串。  
  
1. ![](https://mmbiz.qpic.cn/sz_mmbiz_png/OkRKg4J9smVAsWvMTN50LvGZ1elzfUEc5ibRCSzfMQUG3QAtQ2KoQmhiaC4jVDMAIvSn2bcWxeVBMFdSLia4azic7w/640?wx_fmt=png "")  
  
  
1. 3. 调用链可以看到使用了AngularJS，查看页面源代码并观察您的随机字符串是否包含在 ng-app  
 指令中。  
  
1. ![](https://mmbiz.qpic.cn/sz_mmbiz_png/OkRKg4J9smVAsWvMTN50LvGZ1elzfUEcxQR8vL432VicwibhajEdugVmLiaj5mibY3SkMRsmG9d7ZAJo0aOviaJJVBA/640?wx_fmt=png "")  
  
  
1. ![](https://mmbiz.qpic.cn/sz_mmbiz_png/OkRKg4J9smVAsWvMTN50LvGZ1elzfUEcQYTTBribafM4YTyQQNjJzTicvlg8vjLoIRvcqYF1x9eRfbx7hsicWCAibw/640?wx_fmt=png "")  
  
  
1. 4. 输入以下 AngularJS 表达式：```
{{$on.constructor('alert(1)')()}}
```  
  
  
  
  
   
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/OkRKg4J9smVAsWvMTN50LvGZ1elzfUEc8fyVjdZWHS0uiaj7FK7hp6tGtlRa4EFes9m7vY36iaNM7VrdKKksKQYg/640?wx_fmt=png "")  
  
  
   
  
  
   
  
仅限交流学习使用，如您在使用本工具或代码的过程中存在任何非法行为，您需自行  
承担相应后果，我们将不承担任何法律及连带责任。  
“如侵权请私聊公众号删文”。  
  
