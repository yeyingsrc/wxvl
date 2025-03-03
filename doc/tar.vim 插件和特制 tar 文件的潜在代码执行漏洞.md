#  tar.vim 插件和特制 tar 文件的潜在代码执行漏洞   
 独眼情报   2025-03-03 07:37  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KgxDGkACWnTckKgWhGNx0UrQCqqBvD15S5OpMibzWyQLzGn2OUjaGOJ6oOYrnRKQaVPvbn4fuk4IsCgspicgI9gA/640?wx_fmt=png&from=appmsg "")  
# tar.vim 插件和特制 tar 文件的潜在代码执行漏洞  
  
**日期：**  
 2025年3月2日**严重性：**  
 高**CVE：**  
 未指定**CWE：**  
 输入验证不当 (CWE-20)  
## 摘要  
  
tar.vim 插件和特制 tar 文件存在潜在代码执行漏洞  
## 详细描述  
  
Vim 分发时包含 tar.vim 插件，该插件允许轻松编辑和查看（压缩或未压缩的）tar 文件。  
  
自 2024 年 11 月 11 日的提交 129a844（runtime(tar): 更新 tar.vim 以支持权限）以来，tar.vim 插件使用 ":read " ex 命令行在光标位置下方添加内容，但该命令未经过安全处理，而是直接从 tar 归档文件中获取。这允许通过特制的 tar 归档文件执行 shell 命令。这种漏洞是否真正发生，取决于所使用的 shell（'shell' 选项，通过 $SHELL 设置）。  
## 影响  
  
影响程度**高**  
，但用户必须被说服使用 Vim 编辑此类文件，这会显示文件名，因此谨慎的用户可能会怀疑正在发生一些奇怪的事情。  
  
Vim 项目感谢 RyotaK（GMO Flatt Security Inc）报告此问题。  
  
此问题已在 Vim 补丁 v9.1.0647 中修复。  
  
  
https://github.com/vim/vim/security/advisories/GHSA-wfmf-8626-q3r3  
  
