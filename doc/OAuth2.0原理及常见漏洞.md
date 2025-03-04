#  OAuth2.0原理及常见漏洞   
原创 信安路漫漫  信安路漫漫   2025-03-03 23:01  
  
# 前言  
  
目前，很多的系统在登录的时候都支持通过第三方账号登录，如通过微信，qq，微博扫描登录。这种一般都是通过OAuth2.0搭建完成的。  
  
有一个问题需要思考：通过第三方应用登录的过程是否存在一些安全问题？  
  
本篇文章就来看看OAuth2.0的登录原理以及可能存在的安全问题。  
# OAuth2.0登录原理  
  
Oauth2.0具有多种授权许可机制协议：授权码许可机制、客户端凭据机制、资源拥有者凭据机制（密码模式）和隐式许可机制。  
  
在了解授权许可机制协议之前，我们得需要了解在OAuth 2.0 的体系里面有 4 种角色，按照官方的称呼它们分别是资源拥有者、客户端、授权服务和受保护资源。  
  
资源拥有者（可以指拥有资源的用户）  
  
客户端（可以理解为第三方系统/软件）  
  
授权服务（权限校验和授权系统(认证服务中心)）  
  
受保护资源（用户在系统上所具有的资源/或者能够访问的资源）  
## 授权码许可机制  
  
授权码许可机制的参与者：资源拥有者、客户端、授权服务、受保护资源  
  
授权码模式这种场景下的授权，第三方软件可以通过拿到资源拥有者授权后的授权码，以及注册时的 client_id 和 client_secret 来换回访问令牌 token 的值。  
  
时序图  
  
![1737511235_67905143048199d1429e4.png!small?1737511235573](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrkRicAvsH3W8qIESJQKiaBt96DZpmtdfDEib9UoeFqskReQcLxmDYaWhHQ/640?wx_fmt=jpeg&from=appmsg "")  
  
下面以一个网站来看一下具体流程  
  
1）打开网站，选择使用微信登录页面  
  
![1737511247_6790514f4ca8e117c9c3b.png!small?1737511247896](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrFUt2xVyVVibTSzFkZqQ2aXIgwRSy2Pw6Nict1hbhlr3WG7zxbNoxYsNg/640?wx_fmt=jpeg&from=appmsg "")  
  
发送了下面的请求  
  
![1737511268_6790516427452745f2cb2.png!small?1737511268614](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssr2JsIETFE32zHW0JThBcM7DZE0Sn5IVR6Qaz9DaxA0AfPgLWbbJp8Yg/640?wx_fmt=jpeg&from=appmsg "")  
  
每个参数的含义如下  
- appid：客户端应用程序的唯一标识符的强制参数，此值是在客户端应用程序向OAuth服务注册时生成的。此值唯一，表示来自于那个客户端。  
  
用微信扫描登录时需要再微信开放平台（  
https://open.weixin.qq.com/）进行注册，注册完成以后可以拿到AppID和AppSecret两个。  
  
![1737511283_67905173969fb72733353.png!small?1737511284186](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssr7jYibMb7hOFQIDQlD7LxE5pMvQTWBkx3GiaJnDUOlv81xASM9wRPBLxw/640?wx_fmt=jpeg&from=appmsg "")  
- redirect_uri：向客户端应用程序发送授权代码时用户浏览器应重定向到的URI，这也被称为"回调URI"或回调端点"。在微信开放平台填写。  
  
- response_type：确定客户端应用程序期望的响应类型以及它想要启动的流，对于授权代码授予类型，值应为代码  
  
- scope：用于指定客户端应用程序要访问用户数据的哪个子集，这些可能是由OAuth提供程序设置的自定义作用域，也可能是由OpenIDConnect规范定义的标准化作用域  
  
- state：用于存储与客户端应用程序上的当前会话绑定的唯一、不可更改的值，OAuth服务应该在响应中返回这个确切的值以及授权代码，通过确保对其/callback端点的请求来自发起OAuth流的同一个人，此参数可作为客户端应用程序的CSRF令牌形式![1737511293_6790517dd0da5ab78e85b.png!small?1737511294393](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrFX42p512Z8RVD5iauVczLUn2gbpdettzS7WCIjFVUEZXK51vcDmzAAg/640?wx_fmt=jpeg&from=appmsg "")  
  
  
这里面可能存在的问题：  
- 没有state参数，可能造成什么危害  
  
- redirect_uri是否可以伪造  
  
2）跳转到授权页面，用户手机上扫描二维码  
  
![1737511466_6790522ae46b86a3aa793.png!small?1737511467381](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrbn8NxJdCZY0Z2XMNZgopTcTnMOqt0b6BAdMlFyxLicGKgrNgoqKuiaiaA/640?wx_fmt=jpeg&from=appmsg "")  
  
3）手机上确认以后，会返回一个授权码（问题：扫描后看是否需要人工确认  
）  
  
![1737511484_6790523c58537f74a6027.png!small?1737511484801](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssr4wsC23TDeCUwj6QoHjQ8TViceHwDAFuczkp8Q6rwMRm1Bb6I9aIH8dw/640?wx_fmt=jpeg&from=appmsg "")  
  
返回授权码  
  
![1737511491_67905243ab72692dbf846.png!small?1737511492161](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrMSHGAbOvt9hmz9RQ1ibGUXpENbHJKicBiaTTLOY4GOiakkWiaPXnEEunn8A/640?wx_fmt=jpeg&from=appmsg "")  
  
4）返回授权码以后，前端携带这个授权码到后端，后端就可以与微信端进行交互获取运行的数据  
  
![1737511501_6790524d2ece6edf1bf49.png!small?1737511513913](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrBkc5ib7WLjjaMQibShpic0rWEzwWU9qH75wevkzjCXTJEfaAMrAXkULbQ/640?wx_fmt=jpeg&from=appmsg "")  
  
## 资源拥有者凭据机制（密码模式）  
  
资源拥有者凭据，顾名思义就是资源拥有者的凭据（账号，密码）。在这场景里面就不存在第三方软件这概念，相当于就是访问系统中的一个子系统，他们之间互相信任。举个例子来说就是，腾讯有许多的游戏，你只需要用qq账号密码就可以登录游戏玩，不需要进行腾讯授权。因为该游戏是腾讯旗下的，他们相互信任的，所以不存在第三方的说法。  
  
客户端凭据机制的参与者：资源拥有者、客户端、授权服务、受保护资源  
  
时序图  
  
![1737511900_679053dc13d8791bfb05a.png!small?1737511900667](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrogTGTuDcWxPcdB51urwDftIFyaVqoFIENPYWcE6VAYlak9OA8PZxBA/640?wx_fmt=jpeg&from=appmsg "")  
## 客户端凭据机制  
  
相当于就是第三方软件访问不需要资源拥有者授权的资源和数据，换句话说在这里客户端也可以看作是资源拥有者。举个例子来说就是第三方软件访问一些公共的服务，譬如说一些地图信息，logo图标等。  
  
这种场景下的授权，便是客户端凭据许可，第三方软件可以直接使用注册时的 client_id 和 client_secret 来换回访问令牌 token 的值。  
  
客户端凭据机制的参与者：客户端、授权服务、受保护资源  
  
时序图  
  
![1737511944_679054086377972f056f9.png!small?1737511945077](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrw7pJGvsOw5ZIwX4vwVmN6BK6MYsHaMdZXgibB0c2JEzXOrOPPibREanA/640?wx_fmt=jpeg&from=appmsg "")  
## 隐式许可机制  
  
隐式许可机制的场景适用于没有后端服务的应用，举个例子来说的话就是在浏览器中执行，譬如说JavaScript应用。  
  
在这种情况下，第三方软件对于浏览器就没有任何保密的数据可以隐藏了，也不再需要应用密钥 app_secret 的值了，也不用再通过授权码 code 来换取访问令牌 access_token 的值了。因此，隐式许可授权流程的安全性会降低很多。  
  
这种场景下的授权，第三方软件可以直接使用注册时的 client_id来换回访问令牌 token 的值。  
  
时序图  
  
![1737511962_6790541aa2a4b55c56a6e.png!small?1737511963083](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrp4VY4JmKW7RZUwE9QBGUeK4oRXlghicLKhVsiaajfk4vSUcRep94Daibg/640?wx_fmt=jpeg&from=appmsg "")  
  
隐式授权类型要简单得多。客户端应用程序无需先获取授权码，然后再将其换成访问令牌，而是在用户同意后立即收到访问令牌。  
  
您可能想知道为什么客户端应用程序并不总是使用隐式授权类型。答案相对简单 - 它的安全性要低得多。使用隐式授权类型时，所有通信都通过浏览器重定向进行 - 没有像授权代码流中那样的安全反向通道。这意味着敏感的访问令牌和用户数据更容易受到潜在攻击。  
## OAuth 2.0的安全性考虑  
### 1）重定向URI的安全性  
  
重定向URI是客户端接收授权码和访问令牌的地址。为了防止攻击者拦截这些敏感信息，重定向URI应该使用HTTPS协议。此外，授权服务器应该只接受预先注册的重定向URI，以防止攻击者将用户重定向到恶意网站。  
### 2）访问令牌的保护  
  
访问令牌是一个敏感的凭证，如果被攻击者获取，他们就可以访问用户的资源。因此，访问令牌应该在所有传输过程中使用HTTPS协议进行加密，防止被窃听。在存储访问令牌时，也应该使用适当的加密措施进行保护。  
### 3）刷新令牌的使用和保护  
  
刷新令牌通常有较长的有效期，甚至可以设置为永不过期。因此，如果刷新令牌被攻击者获取，他们就可以持续访问用户的资源。为了防止这种情况，刷新令牌应该只在后端服务中使用，不应该暴露给前端应用。此外，刷新令牌也应该在所有传输和存储过程中进行加密保护。  
### 4）CSRF攻击和防范  
  
CSRF（跨站请求伪造）是一种常见的网络攻击，攻击者通过伪造用户的请求来执行未经授权的操作。为了防止CSRF攻击，OAuth 2.0的授权请求可以包含一个state参数，这是一个随机生成的字符串，用于在授权服务器重定向回客户端时验证请求的合法性。客户端在发送授权请求时生成state参数，并在接收授权响应时验证它，如果不匹配，就拒绝响应。  
# OAuth常见漏洞  
  
上面介绍了OAuth常见的一些模式，下面我们来分析一下，OAuth可能存在的问题。  
### 1）CSRF攻击  
  
该攻击一般出现的场景一般在登陆以后绑定微信的情况下。应用提供了用户账号和微信账号做绑定的功能，也就是说用户先用自己的账号登录，然后可以绑定微信账号，以便后续可以使用微信账号来登录。  
  
场景一：攻击者首先通过自己的微信号获取授权码 code，然后伪造一个链接诱导受害者登录以后点击，那么受害者的账号就会与攻击者的微信号进行绑定，从而可以登录受害者的账号。  
  
![1737511614_679052be18620f60fe11b.png!small?1737511614777](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssr6sS2nicEDlPg8my9gxWtL0TKHD1WoyqdsOSsKIqFVWXFZnohPOIibuiaQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
这个后果可想而知。那如何避免这种攻击呢？方法也很简单，实际上 OAuth 2.0 中也有这样的建议，就是使用 state 参数，它是一个随机值的参数。当应用请求授权码的时候附带一个自己生成 state 参数值，同时授权服务也要按照规则将这个随机的 state 值跟授权码 code 一起返回。这样，当应用接收到授权码的时候，就要在应用这一侧做一个 state 参数值的比对校验，如果相同就继续流程，否则直接拒绝后续流程。  
  
在这样的情况下，软件 A 要想再发起 CSRF 攻击，就必须另外构造一个 state 值，而这个 state 没那么容易被伪造。  
  
这本就是一个随机的数值，而且在生成时就遵从了被“猜中”的概率要极小的建议。  
### 2）XSS攻击  
  
当请求抵达受保护资源服务时，系统需要做校验，比如第三方软件身份合法性校验、访问令牌 access_token 的校验，如果这些信息都不能被校验通过，受保护资源服务就会返回错误的信息。大多数情况下，受保护资源都是把输入的内容，比如 app_id invalid、access_token invalid ，再回显一遍，这时就会被 XSS 攻击者捕获到机会。  
  
试想下，如果攻击者传入了一些恶意的、搜集用户数据的 JavaScript 代码，受保护资源服务直接原路返回到用户的页面上，那么当用户触发到这些代码的时候就会遭受到攻击。  
  
因此，受保护资源服务就需要对这类 XSS 漏洞做修复，而具体的修复方法跟其它网站防御 XSS 类似，最简单的方法就是对此类非法信息做转义过滤  
  
![1737511666_679052f20fd419b894128.png!small?1737511666546](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrSUUrQcZL3AP0Hk9RVlUicXZ3JqPPHOFxXwBv06U6xYJibPuf7IUibwcKg/640?wx_fmt=jpeg&from=appmsg "")  
### 3）授权码失窃  
  
我们知道客户端在获取验证码后，会调用回调地址，然后客户端服务器去获取令牌。那么我们如果可以拿到别人的授权码，是不是也可以伪造请求直接去获取令牌了。  
  
攻击者劫持了授权码之后，想要进一步获取令牌，一般还需要请求上下文state：  
- **保证授权码与请求上下文state一致**  
攻击者只要想办法实现上面两点，就能轻松获取令牌了。为了实现这一点，攻击者拦截用户授权请求，并将客户端重定向uri指向自己的页面，用户完成身份认证和授权，如果授权服务器使用宽松的redirect_uri校验，则会验证通过生成一个授权码，然后带着授权码和state信息重定向到攻击者页面，攻击者拿到授权码code和state后会伪造用户向客户端的请求（调用客户端重定向地址），因为state和code都是受害用户请求的，所以客户端验证通过并向授权服务器请求令牌，授权服务器接收到的请求会被认为是正常请求，校验通过颁发token，客户端获取到token继续请求受保护资源，资源服务器将返回请求资源，但是接收资源的人不是正常用户，而是攻击者！下面看一下这种攻击的实现流程：  
  
![1737511702_67905316b1bbe2a1a4e0f.png!small?1737511703931](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrJ0K5fx9ibmwuMtUiaJWKeOHVewkibu9Vy1XUkXg4gibaibVIref66OzSfFg/640?wx_fmt=jpeg&from=appmsg "")  
  
**看到这里可能有一个疑问，我们如果去获取授权code呢？**  
  
## 4）重定向URL被篡改  
  
有的时候，授权服务提供方并没有对第三方软件的回调 URI 做完整性要求和完整性校验。  
  
比如，第三软件 B 的详细回调 URI 是  
https://app.org/callback  
，那么在完整性校验缺失的情况下，只要以  
https://app.org  
开始或者其他域名的的回调 URI 地址，都会被认为是合法的。  
  
那么我们就可以修改回调地址，然后诱导用户去点击这个链接，这样当返回code的时候就会去回调我们修改后的地址，这样就可以在服务器上看到其code。进而按照授权码失窃来进行攻击  
  
问题：大部分的授权服务器都限制了url的host头，如何进行利用？  
  
第一种方式：结合任意url跳转。如果验证服务器没有完全匹配，可以通过../这种方式改变路径的。可以在页面上找一个任意url跳转漏洞，配合这种漏洞进行利用。  
  
如下图，通过../改变路径，然后将跳转的url指向我们的服务器，这样就可以在服务器的日志上看到想要的内容  
  
![1737511768_6790535830f6da4997d05.png!small?1737511768598](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrGARumK5OiaEvbiaffrRoTYQAxIxvsQpiaEK3gWrChaYmfv4LbNaibgoxsg/640?wx_fmt=jpeg&from=appmsg "")  
  
第二种方式：寻找可以留言的地方。然后将内容发送留言的内容中去获取  
  
![1737511774_6790535e286f83bb83f4e.png!small?1737511774601](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrYgjGJDibZichP1BPHwK512SBvxzwWUibwnUxSYMV7oZNia0WocucJHn0lw/640?wx_fmt=jpeg&from=appmsg "")  
## 5）登录流程设计缺陷  
  
登录流程如果存在设计缺陷也可能会导致接管其它人的账号。如扫描登录时无需人员确认，那么就可以将二维码发送给其他人诱导点击。  
  
## portswigger案例  
  
OAuth 隐式流程绕过身份验证  
  
https://portswigger.net/web-security/oauth/lab-oauth-authentication-bypass-via-oauth-implicit-flow  
  
使用给的用户名和密码登录  
  
username=wiener&password=peter  
  
![1737511794_679053729689610939a62.png!small?1737511795089](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssr4rA0TH7bANMQiaqTNOqVo79Lok5KCGC70zBkFerYMuicqHnYOdFkY9ng/640?wx_fmt=jpeg&from=appmsg "")  
  
这里只校验了token，然后根据email判断登录用户，我们替换email重放数据包就可以登录到其它账号  
  
![1737511804_6790537c9c2adbbeedeff.png!small?1737511807902](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssr0lSaNnYLmrFtPvSZ4ibDTlePdY2S20JTQKXspT1LSicHGJuJoa7NWFmw/640?wx_fmt=jpeg&from=appmsg "")  
  
成功完成  
  
![1737511810_67905382ea3d1770cbbe3.png!small?1737511811529](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Rzo6rPw2nBwIrUr0Iib4XxVV5Vc54mssrsME596MEYHngeV3W9sia87ia7t0knJNOEyt1TIuUDqL6W7mTicHbyiarTw/640?wx_fmt=jpeg&from=appmsg "")  
  
其它的实验内容可以看下面的链接  
  
https://xz.aliyun.com/t/13349  
# 总结  
  
OAuth2.0的漏洞总体上来说就是下面的两种  
  
1）获取到授权code，获取授权code的方式就是未验证回调url，这种方式比较困难  
  
2）就是绑定微信时利用CSRF漏洞，将攻击者的微信绑定到受害者的账号上。这种需要与用户有交互也比较困难。  
  
一个安全的OAuth2.0认证应该做到下面的几点  
- 授权码使用一次之后将其销毁  
  
- 授权服务器应该采用精确匹配的重定向URI校验算法  
  
- 避免让授权服务器成为开放重定向器，在非法的请求中直接返回错误而不是重定向  
  
## 参考链接  
  
https://blog.csdn.net/qq_43284469/article/details/132510807  
  
https://blog.csdn.net/u010020088/article/details/143903471  
  
https://cloud.tencent.com/developer/article/2418791  
  
https://www.cnblogs.com/hellowhy/p/15533625.html  
  
https://xz.aliyun.com/t/13349  
  
  
