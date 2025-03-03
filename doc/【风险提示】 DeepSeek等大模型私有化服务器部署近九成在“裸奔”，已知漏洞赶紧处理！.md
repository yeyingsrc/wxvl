#  【风险提示】 DeepSeek等大模型私有化服务器部署近九成在“裸奔”，已知漏洞赶紧处理！   
 信息安全大事件   2025-03-03 11:19  
  
近期，随着国产大模型 DeepSeek   
的迅速流行，越来越多的企业和个人选择将其进行私有化部署。然而，根据奇安信资产测绘鹰图平台的监测数据，在   
8971   
个运行   
Ollama   
大模型框架的服务器中，有   
6449   
个活跃服务器，其中   
88.9%   
的服务器未采取有效的安全防护措施，处于  
“  
裸奔  
”  
状态。这种状态导致任何人都可以在未经授权的情况下访问这些服务，从而引发数据泄露、服务中断、算力盗用甚至模型文件被删除等严重风险。  
  
**相关组件和系统漏洞**  
  
**1.Ollama工具的安全漏洞**  
  
Ollama   
是一款用于私有化部署大模型的工具，支持   
DeepSeek  
、  
Qwen  
、  
Llama  
、  
Mistral   
等多种语言模型。  
  
默认未设置身份验证和访问控制功能，导致服务   
API   
接口（如  
http://XX.XX.XX.XX:11434  
）可在未经授权的情况下被调用。  
  
2024  
年  
11  
月，研究人员披露了   
Ollama   
框架中的六个安全漏洞，可能被利用执行拒绝服务攻击（  
DDoS  
）、模型污染和模型盗窃等恶意行为。  
  
**2.大模型服务器的安全风险**  
  
服务器未进行必要的安全配置，如未启用防火墙、未设置   
IP   
白名单、未对数据传输进行加密。  
  
已出现通过自动化脚本扫描“裸奔”状态的   
DeepSeek   
服务器，并恶意占用大量计算资源，导致部分用户服务器崩溃的事件。  
<table><tbody style="border-color: rgb(221, 221, 221);word-break: break-all;"><tr style="border-color: rgb(221, 221, 221);word-break: break-all;"><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" align="center" width="105"><span style="font-size: 14px;">漏洞名称</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" align="center" width="85"><span style="font-size: 14px;">风险等级</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" align="center" width="387"><span style="font-size: 14px;">漏洞概述</span></td></tr><tr style="border-color: rgb(221, 221, 221);word-break: break-all;"><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="105"><span style="font-size: 14px;">Ollama 未授权访问风险</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="85"><p style="border-color: rgb(221, 221, 221);word-break: break-all;"><span style="font-size: 14px;">中风险</span><o:p></o:p></p><p style="border-color: rgb(221, 221, 221);word-break: break-all;"><br/></p></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="387"><span style="font-size: 14px;">Ollama 默认未设置身份验证和访问控制功能，其服务 API 接口（如http://XX.XX.XX.XX:11434）可在未授权情况下被调用，攻击者可远程访问该接口，窃取知识库、投喂虚假信息或滥用模型推理资源。</span></td></tr><tr style="border-color: rgb(221, 221, 221);word-break: break-all;"><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="105"><span style="font-size: 14px;">Ollama 模型管理接口风险</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="85"><p style="line-height:18.0000pt;"><span style="font-size: 14px;">中风险</span><span style="font-size: 14px;font-family: 等线;letter-spacing: 0.2pt;"><o:p></o:p></span></p><p style="line-height:18.0000pt;"><br/></p></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="387"><span style="font-size: 14px;">攻击者可通过未授权访问 Ollama 的模型管理接口，读取、下载或删除私有模型文件。</span></td></tr><tr style="border-color: rgb(221, 221, 221);word-break: break-all;"><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="105"><span style="font-size: 14px;">Ollama 模型推理接口风险</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="85"><p style="border-color: rgb(221, 221, 221);word-break: break-all;"><span style="font-size: 14px;">中风险</span><o:p></o:p></p><p style="line-height:18.0000pt;"><br/></p></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="387"><span style="font-size: 14px;">攻击者可利用未授权访问的模型推理接口，执行任意 Prompt 指令，滥用模型推理资源。</span></td></tr><tr style="border-color: rgb(221, 221, 221);word-break: break-all;"><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="105"><span style="font-size: 14px;">Ollama 系统配置接口风险</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="85"><p style="line-height:18.0000pt;"><span style="font-size: 14px;">中风险</span><span style="font-size: 14px;font-family: 等线;letter-spacing: 0.2pt;"><o:p></o:p></span></p><p style="line-height:18.0000pt;"><br/></p></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="387"><span style="font-size: 14px;">攻击者可能通过未授权访问篡改 Ollama 服务参数，影响系统配置。</span></td></tr><tr><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="105"><span style="font-size: 14px;">Ollama 远程代码执行漏洞(CVE-2024-37032)</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="85"><span style="font-size: 14px;">高危（CVSS评分9.1）</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="387"><span style="font-size: 14px;">在 Ollama 0.1.34 之前的版本中存在远程代码执行漏洞，攻击者无需身份验证即可通过接口操控服务器，实现任意代码执行。</span></td></tr><tr><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="105"><span style="font-size: 14px;">Ollama 路径遍历漏洞(CVE-2024-45436)</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="85"><span style="font-size: 14px;">高危（CVSS评分9.1）</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="387"><span style="font-size: 14px;">在 Ollama 0.1.47 之前的版本中，extractFromZipFile函数在处理ZIP文件解压时，未能正确限制文件路径，导致攻击者可以通过特制的ZIP文件将文件解压到父目录之外。</span></td></tr><tr><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="105"><span style="font-size: 14px;">Ollama 信息泄露漏洞(CVE-2024-39722)</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="85"><span style="font-size: 14px;">高危（CVSS评分7.5）</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="387"><span style="font-size: 14px;">在 Ollama 0.1.46 之前的版本中发现了一个漏洞。它通过 api/push 路由中的路径遍历，攻击者可以通过发送包含不存在的文件路径参数的恶意请求，利用该漏洞确定服务器上文件的存在性。</span></td></tr><tr><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="105"><span style="font-size: 14px;">Ollama 信息泄露漏洞(CVE-2024-39719)</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="85"><span style="font-size: 14px;">高危（CVSS评分7.5）</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="387"><span style="font-size: 14px;">Ollama 0.3.14 及之前版本存在信息泄露漏洞，该漏洞源于/api/create端点对路径参数处理不当。攻击者可以通过发送包含不存在的文件路径参数的恶意请求，利用该漏洞确定服务器上文件的存在性，从而可能泄露敏感信息。</span></td></tr><tr><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="105"><span style="font-size: 14px;">Ollama DNS 重绑定漏洞(CVE-2024-28224)</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="85"><span style="font-size: 14px;">高危（CVSS评分8.8）</span></td><td valign="top" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="387"><span style="font-size: 14px;">Ollama 在 0.1.29 版本之前存在一个 DNS 重绑定漏洞，可能允许远程访问完整 API，从而让未经授权的用户与大型语言模型聊天、删除模型或导致拒绝服务（资源耗尽）。</span></td></tr><tr><td valign="top" colspan="1" rowspan="1" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="105"><span style="font-size: 14px;">Ollama 拒绝服务漏洞(CVE-2024-39721)</span></td><td valign="top" colspan="1" rowspan="1" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="85"><span style="font-size: 14px;">高危（CVSS评分7.5）</span></td><td valign="top" colspan="1" rowspan="1" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="387"><span style="font-size: 14px;">Ollama 在 0.1.34 版本之前存在一个拒绝服务漏洞，该漏洞存在于Ollama的/api/create端点中，CreateModelHandler函数未对用户控制的req.Path参数进行适当验证。攻击者可以通过向该端点发送包含特殊文件路径（如/dev/random）的POST请求，使程序进入无限循环，耗尽系统资源，最终导致拒绝服务。</span></td></tr><tr><td valign="top" colspan="1" rowspan="1" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="105"><span style="font-size: 14px;">Ollama 拒绝服务漏洞(CVE-2024-39720)</span></td><td valign="top" colspan="1" rowspan="1" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="85"><span style="font-size: 14px;">高危（CVSS评分8.2）</span></td><td valign="top" colspan="1" rowspan="1" style="border-color: rgb(221, 221, 221);word-break: break-all;" width="387"><span style="font-size: 14px;">在 Ollama 0.1.46 之前的版本中发现了一个漏洞。攻击者可以利用两个 HTTP 请求上传一个仅包含以 GGUF 自定义魔术头开始的 4 个字节的畸形 GGUF 文件。通过利用一个包含指向攻击者控制的 blob 文件的 FROM 语句的自定义 Modelfile，攻击者可以通过 CreateModel 路由使应用程序崩溃，导致段错误。</span></td></tr></tbody></table>  
  
**风险提示**  
  
多家政企单位发出风险提示，指出使用 Ollama   
工具部署   
DeepSeek   
等大模型时，存在以下安全隐患：  
  
1.  
未经授权的访问和调用。  
  
2.  
数据泄露和服务中断。  
  
3.  
模型文件被删除，导致系统不可用。  
  
4.  
算力被非法占用，用于加密货币挖矿或   
DDoS   
攻击。  
  
**防范建议**  
  
**1.配置访问控制**  
  
若  
Ollama   
仅对本地提供服务，建议设置环境变量   
Environment="OLLAMA_HOST=127.0.0.1"  
，限制为仅本地访问。  
  
若需要对外提供服务，可通过修改   
config.yaml   
或   
settings.json   
配置文件限定可调用   
Ollama   
服务的   
IP   
地址，或在防火墙等设备上部署   
IP   
白名单。  
  
  
**2.加强身份认证**  
  
立即修改   
Ollama   
配置，增加身份认证机制。  
  
  
**3.数据加密**  
  
对所有传输的数据进行加密，避免敏感信息泄露。  
  
  
**4.部署安全产品**  
  
使用专业安全工具（如奇安信大模型卫士）防范越狱攻击、提示词注入等风险。  
  
  
**5.定期检查与维护**  
  
定期检查服务器的安全状态，关闭不必要的端口，限制计算资源的使用，并加强监控。  
  
  
**6.升级安全防护手段**  
  
针对大模型特有的安全风险，如提示注入攻击、信息内容安全风险、数据隐私泄漏等，升级安全防护手段。  
  
<table><tbody style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><tr class="ue-table-interlace-color-single js_darkmode__0" data-style="-webkit-tap-highlight-color: transparent; outline: 0px; background-color: rgb(28, 28, 28); visibility: visible; color: rgb(205, 205, 205) !important;" style="-webkit-tap-highlight-color: transparent;outline: 0px;background-color: rgb(28, 28, 28);visibility: visible;color: rgb(205, 205, 205) !important;"><td width="557" valign="top" data-style="-webkit-tap-highlight-color: transparent; outline: 0px; word-break: break-all; hyphens: auto; border-color: rgb(76, 76, 76); background-color: rgb(255, 218, 169); visibility: visible; color: rgb(25, 25, 25) !important;" class="js_darkmode__1" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-color: rgb(76, 76, 76);background-color: rgb(255, 218, 169);visibility: visible;color: rgb(25, 25, 25) !important;"><section style="-webkit-tap-highlight-color: transparent;outline: 0px;line-height: normal;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;font-size: 12px;visibility: visible;color: rgb(0, 0, 0);">尊敬的读者：<br style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"/>感谢您花时间阅读我们提供的这篇文章。我们非常重视您的时间和精力，并深知信息对您的重要性。<br style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"/>我们希望了解您对这篇文章的看法和感受。我们真诚地想知道您是否认为这篇文章为您带来了有价值的资讯和启示，是否有助于您的个人或职业发展。<br style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"/>如果您认为这篇文章对您非常有价值，并且希望获得更多的相关资讯和服务，我们愿意为您提供进一步的定制化服务。请通过填写我们提供的在线表单，与我们联系并提供您的邮箱地址或其他联系方式。我们将定期向您发送相关资讯和更新，以帮助您更好地了解我们的服务和文章内容。</span></section><section style="-webkit-tap-highlight-color: transparent;outline: 0px;line-height: normal;visibility: visible;"><br style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"/></section><section style="-webkit-tap-highlight-color: transparent;outline: 0px;line-height: normal;text-indent: 0em;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;color: rgb(0, 0, 0);">                   </span><img alt="图片" class="rich_pages wxw-img" data-backh="106" data-backw="106" data-cropselx1="0" data-cropselx2="119" data-cropsely1="0" data-cropsely2="119" data-galleryid="" data-imgfileid="100006574" data-ratio="1" data-s="300,640" data-src="https://mmbiz.qpic.cn/sz_mmbiz_png/JqliagemfTA5N8G6ZVujodYTTD7NSaxFG5suXlkibicfoGRzCk6vHhCUBx7ST8b4AxdsFVNNAH4ltePBWX4AxKY0A/640?wx_fmt=other&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1&amp;tp=webp" data-type="png" data-w="1000" style="-webkit-tap-highlight-color: transparent;outline: 0px;font-family: 宋体;font-size: 14px;letter-spacing: 0.578px;text-align: center;visibility: visible !important;width: 119px !important;"/></section><section style="-webkit-tap-highlight-color: transparent;outline: 0px;line-height: normal;text-indent: 0em;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;font-family: 宋体;font-size: 12px;letter-spacing: 0.578px;text-align: center;color: rgb(0, 0, 0);">                               扫描二维码，参与调查</span></section><section style="-webkit-tap-highlight-color: transparent;outline: 0px;line-height: normal;"><br style="-webkit-tap-highlight-color: transparent;outline: 0px;letter-spacing: 0.544px;"/></section></td></tr></tbody></table>  
  
  
**END**  
  
  
  
点击下方，关注公众号  
  
获取免费咨询和安全服务  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/JqliagemfTA5OxIlGh6IbpxrTJHkcY5DZ4O80nevX4Ev7IHvjZfPZDDMxibSVWk4IdYfaYpuhBgz2iaWS5tzXZLJw/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
  
  
  
安全咨询/安全集成/安全运营  
  
专业可信的信息安全应用服务商！  
  
http://www.jsgjxx.com  
  
  
