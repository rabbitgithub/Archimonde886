##  好好学习，天天向上！

# tcpdump抓包
  tcpdump -i eth0 -nn -s0 -v port 80
  -i 指定网卡进行抓包
  -n 和ss一样，表示不解析域名
  -nn 两个n表示端口也是数字，否则解析成服务名
  -s 设置抓包长度，0表示不限制
  -v 抓包时显示详细输出，-vv、-vvv依次更加详细

  加入-A选项将打印ascii ，-X打印hex码
  tcpdump -A -s0 port 80

  抓取特定ip的相关包
  tcpdump -i eth0 host 10.10.1.1tcpdump -i eth0 dst 10.10.1.20

  -w参数将抓取的包写入到某个文件中
  tcpdump -i eth0 -s0 -w test.pcap

  tcpdump支持表达式，还有更加复杂的例子，比如抓取系统中的get,post请求（非https)
  tcpdump -s 0 -v -n -l | egrep -i "POST /|GET /|Host:"
  
  ###WAF
  什么是WAF？WAF是英文"Web Application Firewall"的缩写，中文意思是"Web应用防火墙"，也称为"网站应用级入侵防御系统"。WAF是集WEB防护、网页保护、负载均衡、应用交付于一体的WEB整体安全防护设备。WAF需要部署在Web服务器的前面，串行接入，不仅在硬件性能上要求高，而且不能影响Web服务，所以HA功能、Bypass功能都是必须的，而且还要与负载均衡、Web Cache等Web服务器前的常见的产品协调部署。
  WAF主要技术
  WAF的主要技术是对入侵的检测能力，尤其是对Web服务入侵的检测能力。常见的实现形式包括代理服务、特征识别、算法识别、模式匹配。代理服务代理方式本身是一种安全网关，基于会话的双向代理，中断了用户与服务器的直接连接，适用于各种加密协议，这也是Web的Cache应用中最常用的技术。代理方式有效防止入侵者的直接进入，对DDOS攻击可以抑制，对非预料的“特别”行为也有所抑制。特征识别识别出入侵者是防护它的前提。特征就是攻击者的“指纹”，如缓冲区溢出时的Shellcode，SQL注入中常见的“真表达(1=1)”。应用信息没有“标准”，但每个软件、行为都有自己的特有属性，病毒与蠕虫的识别就采用此方式，麻烦的就是每种攻击都自己的特征，数量比较庞大，多了也容易相象，误报的可能性也大。虽然目前恶意代码的特征指数型地增长，安全界声言要淘汰此项技术，但目前应用层的识别还没有特别好的方式。算法识别特征识别有缺点，人们在寻求新的方式。对攻击类型进行归类，相同类的特征进行模式化，不再是单个特征的比较，算法识别有些类似模式识别，但对攻击方式依赖性很强，如SQL注入、DDOS、XSS等都开发了相应的识别算法。算法识别是进行语义理解，而不是靠“长相”识别。模式匹配IDS中“古老”的技术，把攻击行为归纳成一定模式，匹配后能确定是入侵行为。协议模式是其中简单的，是按标准协议的规程来定义模式，行为模式就复杂一些。最大挑战WAF最大的挑战是识别率，这并不是一个容易测量的指标，因为漏网进去的入侵者，并非都大肆张扬，比如给网页挂马，很难察觉进来的是哪一个，不知道当然也无法统计。对于已知的攻击方式，可以谈识别率；对未知的攻击方式，你也只好等他自己“跳”出来才知道。
  WAF分类
  WAF从形态上可分为硬件WAF、WAF防护软件和云WAF。硬件WAF通常串行部署在Web服务器前端，用于检测、阻断异常流量。通过代理技术代理来自外部的流量，并对请求包进行解析，通过安全规则库的攻击规则进行匹配，如成功匹配规则库中的规则，则识别为异常并进行请求阻断。软件WAF通常部署在需要防护的服务器上，通过监听端口或以Web容器扩展方式进行请求检测和阻断。云WAF云WAF，也称WEB应用防火墙的云模式，这种模式让用户不需要在自己的网络中安装软件程序或部署硬件设备，就可以对网站实施安全防护，它的主要实现方式是利用DNS技术，通过移交域名解析权来实现安全防护。用户的请求首先发送到云端节点进行检测，如存在异常请求则进行拦截否则将请求转发至真实服务器。
  WAF作用
  WAF的作用主要包括WEB防护和防止WEB信息泄露两大部分，具体如下：Web防护
  1.网络层：DDOS攻击、Syn Flood、Ack Flood、Http/Https Flood（CC攻击）、慢速攻击；
  2.应用层：URL黑白名单、HTTP协议规范（包括特殊字符过滤、请求方式、内容传输方式，例如：multipart/form-data，text/xml，application/x-www-form-urlencoded）；
  3.注入攻击（form和URL参数，post和get）：SQL注入防御、 LDAP注入防御、 命令注入防护（OS命令，webshell等）、 XPath注入、 Xml/Json注入、XSS攻击（form和URL参数，post和get，现阶段分为三类攻击：存储式(危害大，也是一种流行方式)，反射式、基于Dom的XSS）；
  4.目录遍历（Path Traversal）
  5.form表单数据验证和表单篡改和注入（表单验证银行卡、数据、日期等）
  6.认证管理和会话劫持（cookie加密：防护会话劫持，包括cookie超时）。
  7.内容过滤（这儿强调上传内容过滤post form和get 参数，主要应用论坛）
  8.Web服务器漏洞探测（apache版本等隐藏，站点隐藏）
  9.爬虫防护（基于SRC IP，周期判断访问数，爬虫白名单除外）
  10.CSRF（Cross-site request forgery）（WAF采用token方式处理能够解决）
  11.篡改（包括盗链）（WAF周期爬服务器网页，进行对比验证，如果篡改发现篡改，Client访问WAF网页）
  12Web服务器漏洞扫描（模拟攻击，判断缺陷，自动配置对应规则）
  13.cache加速（静态页面优化，PDF，图片等，需要周期映像）
  14.错误码过滤（探测服务，及其目录结构）
  15.站点转换（URL rewrite）
  16.发现攻击锁定（发现攻击，锁定用户）
  17.查杀毒
  18.加密传输（http -> https转化，即client-waf之间通过https，waf与server之间http）。URL ACL（URL匹配一些规则）。防止Web信息泄露银行卡（信用卡、借记卡）、社保卡、驾照等，采用覆盖和隐藏两种方式。敏感词过滤、Web中关键词（政治敏感词、技术关键词等）防止文件泄露（word、pdf等扩展文件及其关键词），Web服务器上的文件。

You can use the [editor on GitHub](https://github.com/rabbitgithub/Archimonde886/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/rabbitgithub/Archimonde886/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
