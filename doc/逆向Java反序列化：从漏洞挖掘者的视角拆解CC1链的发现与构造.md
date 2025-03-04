#  逆向Java反序列化：从漏洞挖掘者的视角拆解CC1链的发现与构造   
M0onPu15e  亿人安全   2025-03-04 01:31  
  
## 前言  
  
Apache Commons Collections 是一个开源的 Java 工具类库，属于 Apache Commons 项目的一部分。它提供了许多扩展和增强标准 Java 集合框架的功能，帮助开发者更高效地处理集合操作。  
## 环境准备  
- CommonsCollections <= 3.2.1  
  
- jdk1.8.0_65(JDK版本必须在1.8.0_71以下)  
  
附带CC的jar文件和源代码的下载地址  
  
https://commons.apache.org/proper/commons-collections/  
  
记得在pom.xml里添加依赖  
```
```  
## CC1链调用过程  
```
```  
## CC链的调用梳理  
  
在org/apache/commons/collections/functors存在InvokerTransformer类，该类的定义如下：  
```
```  
  
可见该类实现了Serializable所以可被反序列化和反序列化。在此类中存在Tranformer方法，cc1链梦开始的地方。  
  
对Tranformer方法进行审计，我们发现，Tranformer方法通过反射机制动态调用传入对象的指定方法（方法名和参数类型由类属性指定），并返回方法的执行结果；如果传入对象为null  
，则直接返回null  
。  
  
![1740998307_67c586a3c9b93789210c5.png!small?1740998308363](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVglmKoc7D7yZugKgkWUHkaT5s7Rb9EjI6768dg3iaJnSVWrQIrFlHhAvg/640?wx_fmt=jpeg&from=appmsg "")  
  
那么如果我们直接传入Runtime  
对象，并将InvokerTransformer类的类属性设置为执行命令的参数，不就可以达到命令执行的效果了嘛。  
  
于是我们开始验证  
```
```  
  
![1740998328_67c586b88c90888423644.png!small?1740998329016](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgJKcBT9wQUgf4E0VvdjsL9mgMzTDOJwB8ZSeFc98ibyQ5eyPWl4NwjMQ/640?wx_fmt=jpeg&from=appmsg "")  
  
代码执行的结果表明，InvokerTransfomer类的transform方法完全可以作为漏洞执行的终点。  
  
下一步我们就要查找哪里调用了InvokerTransfomer类的transform方法。  
  
![1740998344_67c586c81937883292849.png!small?1740998344783](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgiaibvpicMFoMEPCtyj7TdeG2xGgicSjrA4HL1NzvvTOFnL25gNqKOtJrkQ/640?wx_fmt=jpeg&from=appmsg "")  
  
说实话真的好多，最后在TransformedMap类里边的checkSetValue（）通过valueTransformer调用了transform。  
  
![1740998357_67c586d5cd7175c39b351.png!small?1740998358151](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgDR10svlXLRrYLU1hfzKFnCpA8Riah4CtcicyKSqUSqczfMX64Nics7HXw/640?wx_fmt=jpeg&from=appmsg "")  
  
而valueTransformer来源于TransformedMap类的构造方法，因此我们就可以控制调用类，执行其transform方法。  
  
但是有一个问题，TransformedMap类的构造方法修饰符为protected。  
  
![1740998373_67c586e5df665a1255296.png!small?1740998374410](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVg9UPpfDJQYLHZlpwIzbe24jb1libaiavUmLJW0pHhGkicWRuU0K9SOV03Q/640?wx_fmt=jpeg&from=appmsg "")  
  
说明外部无法调用，那么只能通过内部调用。于是我们在本类内部找到了两处调用构造方法的方法。  
  
![1740998387_67c586f30508dc165e9b5.png!small?1740998387486](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgibhq7sQvQQK4hMvD8tkPcugpDUXdCIQhQiaTEHO8tHqFwK6SRVYnUnIA/640?wx_fmt=jpeg&from=appmsg "")  
  
![1740998397_67c586fd91a97d0d4e797.png!small?1740998397944](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgRnQvgf7HA4ibgwK28UPm9FsD11xFZUIw0Hia5TFoCu9YlvTFVIYcZ09A/640?wx_fmt=jpeg&from=appmsg "")  
  
![1740998406_67c587063b7b3cf4d77ad.png!small?1740998407443](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgiaPj58gQQFB5hor6SXhncbwU7YKoqvIuR5LHAdYLEk6stj9eScvWtkQ/640?wx_fmt=jpeg&from=appmsg "")  
  
我们优先使用TransformedMap#decorate（因为比较简单）  
  
通过TransformedMap#decorate实例化TransformedMap对象  
```
```  
  
调用checkSetvalue方法  
  
![1740998422_67c58716bdf60520a03d4.png!small?1740998423257](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgCOaonA2DBf7H0UNjelvnGibicv7ibRxwLgB14xgM3kOW2lyXhhsicBRSXw/640?wx_fmt=jpeg&from=appmsg "")  
  
这里显示报错，原因是TransformedMap#decorate返回map对象，所以transformedMap是由Map声明，所以不能直接调用checkSetvalue。只能再找哪里调用了checkSetValue。  
  
![1740998433_67c58721ef68b9536428a.png!small?1740998434620](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgkku2WXKylth0KXC7tIaItGq0wVYP0ykWR0CJ2nPDNTBNmdcZq2J3tA/640?wx_fmt=jpeg&from=appmsg "")  
  
可以看到AbstractInputCheckedMapDecorator类下边的静态内部类MapEntry中的setValue调用了checkSetValue。注意静态类不必实例化，可以直接由当前类调用的类。另外AbstractMapDecorator 是一个抽象类，为装饰一个 Map 提供了基类，AbstractInputCheckedMapDecorator 继承自它，而 TransformedMap 又继承自 AbstractInputCheckedMapDecorator。  
  
  
所以，此时可以构造以下代码，实现调用。说明这一段链条成立  
```
```  
  
![1740998451_67c5873324d092b991247.png!small?1740998451726](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgVHLYMNTEoqH2iaQO3JYVia33icjzT0q88c0yNbAfO9Au9iciblKpk3oOialg/640?wx_fmt=jpeg&from=appmsg "")  
  
注意：entrySet()的源头来自父类AbstractMapDecorator的默认实现。  
  
接下来寻找谁在调用AbstractMapDecorator类下的静态类的setValue（），最后在JDK内置的对象sun.reflect.annotation.AnnotationInvocationHandler中重写的readObject方法发现在调用setValue方法  
  
![1740998461_67c5873d845e40080097b.png!small?1740998462119](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgbhd48pgpDcvneSmFumKz91DcnFTcGOTq18HWiaqTMVe7Fwib3c0aLJZw/640?wx_fmt=jpeg&from=appmsg "")  
  
![1740998469_67c58745a073033041366.png!small?1740998470091](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgeALYxPChGuiaicxkenf23FcCiaDpopfX6jEOsl23dtD3INGnsHJwCNUFw/640?wx_fmt=jpeg&from=appmsg "")  
  
而且AnnotationInvocationHandler类继承了Serializeble接口。  
  
如果这条调用链成立，那么我们直接将AnnotationInvocationHandler的序列化即可完成操作，并且通过readObject在反序列化就可以实现链条的调用。  
  
接下来我们进行尝试：  
  
发现AnnotationInvocationHandler访问修饰符为default，在外部无法直接被实例化，所以只能尝试使用反射进行实例化操作。而且  
  
我们来看下AnnotationInvocationHandler的构造方法  
  
![1740998480_67c58750ebc2a7166e33c.png!small?1740998481371](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgD7wef2HSOJcnO9TnlR36niaEZw9Zy6fvcGibAibxlJZcJSqYw7tjyw1Xw/640?wx_fmt=jpeg&from=appmsg "")  
构造函数参数要接受两个参数值，一个注解类型，一个map，另外，我们注意到调用点memberValue来源于AnnotationInvocationHandler的类属性memberValues。  
  
于是写出以下序列化代码：  
```
```  
  
接着我们进行反序列化操作，发现并不能弹出计算机。  
  
![1740998496_67c5876063ef3a628752c.png!small?1740998496974](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVg2Yn4e514o6H3feEIfArO4EExw62ujIuvRDKufVzfgHD7pVMNqvj8DQ/640?wx_fmt=jpeg&from=appmsg "")  
  
我们动调来确定是什么原因引起的：  
  
发现代码无法执行到setValue方法处。  
  
![1740998510_67c5876e1e3baaf658822.png!small?1740998511115](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgsictU38JAWaXib4zqNn4JiaPhIGxl04g1dT0HUh2vRgSQrgJID3uaDNnA/640?wx_fmt=jpeg&from=appmsg "")  
  
memberType 来源于通过调用 AnnotationType.getInstance(type) 获取的 AnnotationType 对象中的 memberTypes() 映射，包含将注解成员名称。（注解学习链接：  
https://www.cnblogs.com/wobushitiegan/p/12460575.html  
）  
  
而Override 注解没有任何成员方法，所以通过 AnnotationType.getInstance(Override.class) 得到的 memberTypes 映射为空。  
  
![1740998522_67c5877a789904cd097a6.png!small?1740998522812](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgkibZKFDZyNicIvgFJuJwbj09ib4OsLHQ1TicGaYuDVTv5WVhBLoQqKzC3g/640?wx_fmt=jpeg&from=appmsg "")  
  
因此，我们需要找到有成员方法的注解。就比如Target注解  
  
![1740998531_67c58783502323dae4ae8.png!small?1740998531678](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgroXnSrtDGZoujEPll6UbibFed9nibWFrfMATjIlRSd7tiavVG5micHjfUA/640?wx_fmt=jpeg&from=appmsg "")  
  
然后我们继续调试，很遗憾，依旧不行  
  
![1740998553_67c587998efc4c26aa31a.png!small?1740998554127](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgGQFY9BOlhvG42TlU46ktQTla4JEJRp0ewyfs27xZoh6aBgbdHVROrg/640?wx_fmt=jpeg&from=appmsg "")  
  
但是我们注意到memberTypes为hashmap，hashmap的键名为value，值为那一串。所以name的值应该为value（此时为key）。  
  
而name是通过调用 memberValue.getKey() 获得的， memberValue 来自于对 memberValues.entrySet() 的遍历。所以我们仅需要修改在我们前面构造的map，将键名改为value，值随意。  
  
![1740998542_67c5878ecb23571d5bf9e.png!small?1740998543487](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgfDmUOuTk9OjjiceetfDPiaKweriavKjHQuic8NHWj1dy9ibTYpD7buDO44g/640?wx_fmt=jpeg&from=appmsg "")  
  
于是代码修改如下  
```
```  
  
我们继续动调  
  
![1740998620_67c587dc674df7c7e399a.png!small?1740998620896](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVg9VX5AuvlLLxXP5Ip4HoViaMzXP1h1DZen94AwKOF6ZPXSpicBJj3iafrg/640?wx_fmt=jpeg&from=appmsg "")  
  
已经成功进入。但是仍旧没弹计算器，甚至还会抛异常。究其原因，是执行过程中，到达漏洞触发点发现并没有将Runtime对象传进来  
  
![1740998629_67c587e579543e568e52d.png!small?1740998630143](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgBog2no5BswXqB1ZLib8QibadvNE1ibQtAYOfkQRia3ynmHg60e2QjsPZbw/640?wx_fmt=jpeg&from=appmsg "")  
  
![1740998637_67c587ed7a587dc41acea.png!small?1740998637860](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgcUOWApklcHmhsVZhoEtvl7ibvF3MHmlDgsjrwz8LdThdThpyKvdD6nA/640?wx_fmt=jpeg&from=appmsg "")  
  
我们对此进行分析，是因为在AnnotationInvocationHandler的452行，出现了 new AnnotationTypeMismatchExceptionProxy 代码，写死了传入的类。  
  
![1740998648_67c587f853f777c9fff9a.png!small?1740998648751](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgnEyEmvI7G6bSZo6VLpeHvBwzMS0aa7E8ojgjyUO3yVAwqoq9ps338A/640?wx_fmt=jpeg&from=appmsg "")  
  
那么如何解决呢，核心思路就是替换AnnotationTypeMismatchExceptionProxy类实例为Runtime对象。这里不得不佩服构造出cc1链的人，找到了ConstantTransformer  
  
这个类的transform很有意思  
```
```  
  
无论传入什么类型，都只返回iConstant，而iConstant是ConstantTransformer类的属性。只要我们将类属性设置为Runtime，不就可以完成cc1链的调用了么。  
  
但是最后还有一个问题，在未进行序列化之前的构造链的Runtime对象是我们自己new出来的，但是我们查看Runtime类，发现并没有实现Serializable，所以不可被反序列化和反序列化。  
  
![1740998724_67c58844b670d18c4c450.png!small?1740998725271](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgBwOwrSCcalWbSbWx77siaNzE463Ficeib38WalIXDXc0iaXia7Yj0UeanDQ/640?wx_fmt=jpeg&from=appmsg "")  
  
但是类的原型class是可以被序列化的，因此我们需要通过Class入手，构造出来Runtime对象，也就是通过反射。  
  
![1740998737_67c58851c11036ab0238b.png!small?1740998738207](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgH4DdNQdx5y9oAPicJV4q9aR9n5QCl9nl8ric1GqfLZu8kkO0NV2ltZAw/640?wx_fmt=jpeg&from=appmsg "")  
  
代码如下：  
  
```
```  
  
可以执行  
  
![1740998750_67c5885e29f4d19e1f7c0.png!small?1740998750723](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgmxy7RAB7iaPZDJ43NwDlFLVW9VClxtrHBXaya6IZjeHDZtSqiaj99eCA/640?wx_fmt=jpeg&from=appmsg "")  
  
然后我们将其改成通过InvokerTransformer反射调用。  
```
```  
  
![1740998770_67c58872ec80997ee7d9e.png!small?1740998771698](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgficiapyuMN7TbOUhibAnlHGhLYW2ttTIPw3Xy4HkQ9BWck0bmqyXcom6w/640?wx_fmt=jpeg&from=appmsg "")  
  
该代码中分别创建了三个 InvokerTransformer 对象来依次调用，无法将多个操作连贯地组合成一个整体。  
  
我们观察以上代码像不像链式调用，也就是上一步的输出作为下一步的输入。而且在实际利用过程中，我们需要一个延迟执行且能够序列化传输的 gadget 链，这时也就引入了另外一个类ChainedTransformer，该类也存在transform方法，代码及说明如下：  
```
```  
  
按照以上代码的思路，假如如果我们给iTransformer赋值为ConstantTransformer就会调用ConstantTransformer的transform，如果给iTransformer赋值为InvokerTransformer就会调用InvokerTransformer的transform，那么通过将 ConstantTransformer 和 InvokerTransformer 组合到 ChainedTransformer 中，可以构建一个转换链，实现复杂的对象转换或方法调用。例如，首先使用 ConstantTransformer 返回 Runtime.class，然后通过一系列 InvokerTransformer 调用 getMethod、invoke 和 exec 方法，最终执行系统命令。  
  
于是生成了我们最终的序列化调用链：  
```
```  
  
我们动调，看下具体的反序列化过程：  
  
在TransformedMap类中valueTransformer值为ChainedTransformer，所以下一步去调用ChainedTransformer#transforme  
  
![1740998804_67c58894bf5d6ec5bbff4.png!small?1740998806145](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgia9xhSibWWyYicAvZjtGibQ7K7JFdbxmn0ibiboDfyRFriczKCh37BOh6n7IQ/640?wx_fmt=jpeg&from=appmsg "")  
  
![1740998815_67c5889fbb3627d0be086.png!small?1740998816366](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgT8DibufpK0GgXQIlDTuInTG53ZBDIEC7ZLot6w2MriaDTCn2BkdSqbAA/640?wx_fmt=jpeg&from=appmsg "")  
  
第一轮，是AnnotationTypeMismatchExceptionProxy类  
  
![1740998825_67c588a95a6afd5f39852.png!small?1740998825995](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgT8DibufpK0GgXQIlDTuInTG53ZBDIEC7ZLot6w2MriaDTCn2BkdSqbAA/640?wx_fmt=jpeg&from=appmsg "")  
  
调用ConstantTransformer#transformer，传入的是AnnotationTypeMismatchExceptionProxy，返回的是Runtime  
  
![1740998836_67c588b404b3a52e4d09a.png!small?1740998836525](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgOD4pwSmElQeeZ30z1xAG2RwLCKUHib3TJXW2tDGaNibMW8dgM4M4Mkzw/640?wx_fmt=jpeg&from=appmsg "")  
  
获取Runtime的getRuntime方法  
  
![1740998849_67c588c1bff5eb4c6f6c1.png!small?1740998850302](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgWhxJ7J6psRCJdbZam1SeuicTawbHrd9PSrOfBAuDoiasTZwcRZwvpGQA/640?wx_fmt=jpeg&from=appmsg "")  
  
最后获取实例，弹出计算器  
  
![1740998860_67c588cc28ef6c1561ba8.png!small?1740998860729](https://mmbiz.qpic.cn/mmbiz_jpg/iar31WKQlTTpDibGmbIWDQWr7R0IqSDgVgrdewDdQ712EyWZEQk3cvQuNIx2oO4NQrkeiatLy4568duSTjUO8tKicg/640?wx_fmt=jpeg&from=appmsg "")  
  
CC1链构造完毕。  
  
  
原文链接：https://www.freebuf.com/articles/network/423349.html  
  
  
  
  
