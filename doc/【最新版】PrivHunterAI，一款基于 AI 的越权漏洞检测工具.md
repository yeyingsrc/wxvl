#  【最新版】PrivHunterAI，一款基于 AI 的越权漏洞检测工具   
Ed1s0nZ  闪石星曜CyberSecurity   2025-03-03 09:37  
  
# PrivHunterAI  
  
本工具通过被动代理方式调用Kimi、DeepSeek、混元和通义千问AI，实现越权漏洞检测。检测能力基于对应AI引擎的API实现，且支持HTTPS协议。  
## 时间线  
- 2025.02.06  
  
- ⭐️新增DeepSeek AI引擎来检测越权；  
  
- 2025.02.18  
  
- ⭐️新增扫描失败重试机制，避免出现漏扫；  
  
- ⭐️新增响应Content-Type白名单，静态文件不扫描；  
  
- ⭐️新增限制每次扫描向AI请求的最大字节，避免因请求包过大导致扫描失败。  
  
- 2025.02.25 -02.27  
  
- ⭐️新增对URL的分析（初步判断是否可能是无需数据鉴权的公共接口）；  
  
- ⭐️新增通义千问 AI引擎来检测越权;  
  
- ⭐️新增前端结果展示功能。  
  
- ⭐️新增针对请求B添加其他headers的功能（适配有些鉴权不在cookie中做的场景）。  
  
- 2025.03.01  
  
- 优化Prompt，降低误报率；  
  
- 优化重试机制，重试会提示类似:AI分析异常，重试中，异常原因： API returned 401: {"code":"InvalidApiKey","message":"Invalid API-key provided.","request_id":"xxxxx"}  
，每10秒重试一次，重试5次失败后放弃重试（避免无限重试）.  
  
- 2025.03.03  
  
- 💰成本优化：在调用 AI 判断越权前，新增鉴权关键字（如 “暂无查询权限”“权限不足” 等）过滤环节，若匹配到关键字则直接输出未越权结果，节省 AI tokens 花销，提升资源利用效率;  
  
- ⭐️新增混元 AI引擎来检测越权;  
  
## 工作流程  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/FFcgFn6WVc3G4gTO9a5iaDRln164PcLX2h4GVUmpSzWHicKNPY0G4GYhgAoLiaWLtkhUPv5S0ACAicShTCQfu9IMJQ/640?wx_fmt=png&from=appmsg "")  
  
## Prompt  
```
ounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(lineounter(line
{
  "role": "你是一个AI，负责通过比较两个HTTP响应数据包来检测潜在的越权行为，并自行做出判断。",
  "inputs": {
    "reqA": "原始请求A",
    "responseA": "账号A请求URL的响应。",
    "responseB": "使用账号B的Cookie（也可能是token等其他参数）重放请求的响应。",
    "statusB": "账号B重放请求的请求状态码。",
    "dynamicFields": ["timestamp", "nonce", "session_id", "uuid", "request_id"]
  },
  "analysisRequirements": {
    "structureAndContentComparison": {
      "urlAnalysis": "结合原始请求A和响应A分析，判断是否可能是无需数据鉴权的公共接口（不作为主要判断依据）。",
      "responseComparison": "比较响应A和响应B的结构和内容，忽略动态字段（如时间戳、随机数、会话ID、X-Request-ID等），并进行语义匹配。",
      "httpStatusCode": "对比HTTP状态码：403/401直接判定越权失败（false），500标记为未知（unknown），200需进一步分析。",
      "similarityAnalysis": "使用字段对比和文本相似度计算（Levenshtein/Jaccard）评估内容相似度。",
      "errorKeywords": "检查responseB是否包含 'Access Denied'、'Permission Denied'、'403 Forbidden' 等错误信息，若有，则判定越权失败。",
      "emptyResponseHandling": "如果responseB返回null、[]、{}或HTTP 204，且responseA有数据，判定为权限受限（false）。",
      "sensitiveDataDetection": "如果responseB包含responseA的敏感数据（如user_id、email、balance），判定为越权成功（true）。",
      "consistencyCheck": "如果responseB和responseA结构一致但关键数据不同，判定可能是权限控制正确（false）。"
    },
    "judgmentCriteria": {
      "authorizationSuccess (true)": "如果不是公共接口，且responseB的结构和非动态字段内容与responseA高度相似，或者responseB包含responseA的敏感数据，则判定为越权成功。",
      "authorizationFailure (false)": "如果是公共接口，或者responseB的结构和responseA不相似，或者responseB明确定义权限错误（403/401/Access Denied），或者responseB为空，则判定为越权失败。",
      "unknown": "如果responseB返回500，或者responseA和responseB结构不同但没有权限相关信息，或者responseB只是部分字段匹配但无法确定影响，则判定为unknown。"
    }
  },
  "outputFormat": {
    "json": {
      "res": "\"true\", \"false\" 或 \"unknown\"",
      "reason": "清晰的判断原因，总体不超过50字。"
    }
  },
  "notes": [
    "仅输出 JSON 格式的结果，不添加任何额外文本或解释。",
    "确保 JSON 格式正确，便于后续处理。",
    "保持客观，仅根据响应内容进行分析。",
    "优先使用 HTTP 状态码、错误信息和数据结构匹配进行判断。",
    "支持用户提供额外的动态字段，提高匹配准确性。"
  ],
  "process": [
    "接收并理解原始请求A、responseA和responseB。",
    "分析原始请求A，判断是否是无需鉴权的公共接口。",
    "提取并忽略动态字段（时间戳、随机数、会话ID）。",
    "对比HTTP状态码，403/401直接判定为false，500标记为unknown。",
    "检查responseB是否包含responseA的敏感数据（如user_id、email），如果有，则判定为true。",
    "检查responseB是否返回错误信息（Access Denied / Forbidden），如果有，则判定为false。",
    "计算responseA和responseB的结构相似度，并使用Levenshtein编辑距离计算文本相似度。",
    "如果responseB内容为空（null、{}、[]），判断可能是权限受限，判定为false。",
    "根据分析结果，返回JSON结果。"
  ]
}
```  
## 使用方法  
1. 下载源代码；  
  
1. 编辑根目录下的config.json  
文件，配置AI  
和对应的apiKeys  
（只需要配置一个即可）；（AI的值可配置qianwen、kimi 或 deepseek） ；  
  
1. 配置headers2  
（请求B对应的headers）；可按需配置suffixes  
、allowedRespHeaders  
（接口后缀白名单，如.js）；  
  
1. 执行go build  
编译项目，并运行二进制文件；  
  
1. 首次启动后需安装证书以解析 HTTPS 流量，证书会在首次启动命令后自动生成，路径为 ~/.mitmproxy/mitmproxy-ca-cert.pem。安装步骤可参考 Python mitmproxy 文档：  
About Certificates  
。  
  
1. BurpSuite 挂下级代理 127.0.0.1:9080  
（端口可在mitmproxy.go  
 的Addr:":9080",  
 中配置）即可开始扫描；  
  
1. 终端和web界面均可查看扫描结果，前端查看结果请访问127.0.0.1:8222  
 。  
  
### 配置文件介绍（config.json）  
<table><thead><tr style="border: 0;border-top: 1px solid #ccc;background-color: white;"><th style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;font-weight: bold;background-color: #f0f0f0;"><section><span leaf="">字段</span></section></th><th style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;font-weight: bold;background-color: #f0f0f0;"><section><span leaf="">用途</span></section></th><th style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;font-weight: bold;background-color: #f0f0f0;"><section><span leaf="">内容举例</span></section></th></tr></thead><tbody><tr style="border: 0;border-top: 1px solid #ccc;background-color: white;"><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><code><span leaf="">AI</span></code></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">指定所使用的 AI 模型</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><code><span leaf="">qianwen</span></code><section><span leaf="">、</span><code><span leaf="">kimi</span></code><span leaf="">、</span><code><span leaf="">hunyuan</span></code><span leaf=""> 或 </span><code><span leaf="">deepseek</span></code></section></td></tr><tr style="border: 0;border-top: 1px solid #ccc;background-color: #F8F8F8;"><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><code><span leaf="">apiKeys</span></code></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">存储不同 AI 服务对应的 API 密钥 （填一个即可，与AI对应）</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">- </span><code><span leaf="">&#34;kimi&#34;: &#34;sk-xxxxxxx&#34;</span></code><br/><span leaf="">- </span><code><span leaf="">&#34;deepseek&#34;: &#34;sk-yyyyyyy&#34;</span></code><br/><span leaf="">- </span><code><span leaf="">&#34;qianwen&#34;: &#34;sk-zzzzzzz&#34;</span></code><br/><span leaf="">- </span><code><span leaf="">&#34;hunyuan&#34;: &#34;sk-aaaaaaa&#34;</span></code></section></td></tr><tr style="border: 0;border-top: 1px solid #ccc;background-color: white;"><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><code><span leaf="">headers2</span></code></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">自定义请求B的 HTTP 请求头信息</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">- </span><code><span leaf="">&#34;Cookie&#34;: &#34;Cookie2&#34;</span></code><br/><span leaf="">- </span><code><span leaf="">&#34;User-Agent&#34;: &#34;PrivHunterAI&#34;</span></code><br/><span leaf="">- </span><code><span leaf="">&#34;Custom-Header&#34;: &#34;CustomValue&#34;</span></code></section></td></tr><tr style="border: 0;border-top: 1px solid #ccc;background-color: #F8F8F8;"><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><code><span leaf="">suffixes</span></code></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">需要过滤的文件后缀名列表</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><code><span leaf="">.js</span></code><section><span leaf="">、</span><code><span leaf="">.ico</span></code><span leaf="">、</span><code><span leaf="">.png</span></code><span leaf="">、</span><code><span leaf="">.jpg</span></code><span leaf="">、 </span><code><span leaf="">.jpeg</span></code></section></td></tr><tr style="border: 0;border-top: 1px solid #ccc;background-color: white;"><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><code><span leaf="">allowedRespHeaders</span></code></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">需要过滤的 HTTP 响应头中的内容类型（</span><code><span leaf="">Content-Type</span></code><span leaf="">）</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><code><span leaf="">image/png</span></code><section><span leaf="">、</span><code><span leaf="">text/html</span></code><span leaf="">、</span><code><span leaf="">application/pdf</span></code><span leaf="">、</span><code><span leaf="">text/css</span></code><span leaf="">、</span><code><span leaf="">audio/mpeg</span></code><span leaf="">、</span><code><span leaf="">audio/wav</span></code><span leaf="">、</span><code><span leaf="">video/mp4</span></code><span leaf="">、</span><code><span leaf="">application/grpc</span></code></section></td></tr><tr style="border: 0;border-top: 1px solid #ccc;background-color: #F8F8F8;"><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><code><span leaf="">respBodyBWhiteList</span></code></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">鉴权关键字（如暂无查询权限、权限不足），用于初筛未越权的接口</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">- </span><code><span leaf="">参数错误</span></code><br/><span leaf="">- </span><code><span leaf="">数据页数不正确</span></code><br/><span leaf="">- </span><code><span leaf="">文件不存在</span></code><br/><span leaf="">- </span><code><span leaf="">系统繁忙，请稍后再试</span></code><br/><span leaf="">- </span><code><span leaf="">请求参数格式不正确</span></code><br/><span leaf="">- </span><code><span leaf="">权限不足</span></code><br/><span leaf="">- </span><code><span leaf="">Token不可为空</span></code><br/><span leaf="">- </span><code><span leaf="">内部错误</span></code></section></td></tr></tbody></table>## 输出效果  
  
持续优化中，目前输出效果如下：  
  
1. 终端输出：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/FFcgFn6WVc3G4gTO9a5iaDRln164PcLX2YiaqdEw2Y51dzwjjL9maD8cxzLRRzrC1VWXzh5D9AQTtZSMQbKmwVKQ/640?wx_fmt=png&from=appmsg "")  
  
2. 前端输出（访问127.0.0.1:8222）：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/FFcgFn6WVc3G4gTO9a5iaDRln164PcLX2hG0097Cprzne6kqCVeicbq8Mp013h4Js4kbncjAyM2wMUPgw5kib0Ffw/640?wx_fmt=png&from=appmsg "")  
  
# 注意  
  
声明：仅用于技术交流，请勿用于非法用途。  
  
  
