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
