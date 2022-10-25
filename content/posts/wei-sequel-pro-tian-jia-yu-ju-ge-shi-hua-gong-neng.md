---
title: 为 Sequel Pro 添加语句格式化功能
date: 2019-04-01
---

在Mac平台上，MySQL客户端有官方的 `MySQL Workbench` 、Jetbrain的 `DataGrip` 、开源的 `Sequel Pro` 。


这三个软件每个我都用过一段时间，前两者太过臃肿，体验不是太好，后者体验好一些，无奈没有代码格式化的体验，好在此软件 `Bundle` 模块拥有 `Format` 功能，可以通过自定义脚本来实现格式化。


## 添加步骤


### 1. 环境准备


Python3

sqlparse模块  （使用 `pip install sqlparse` 来安装模块)


### 2. 添加脚本


点击菜单 `Bundles` -> `Bundle Editor`

展开 `Input Field` 选项到 `Format` 层级

点击+号添加一项配置

按下图配置和修改代码，点击 Click to record shortcut 按钮来设置快捷键。


![WX20200622-102249@2x](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/WX20200622-102249@2x.png)


```python
#!/Users/peiel/.pyenv/shims/python
#coding=utf-8
import sqlparse
import sys
sql = sys.stdin.read()
statements = sqlparse.split(sql)
first = statements[0]
print(sqlparse.format(first, reindent=True, keyword_case='upper'), end = '')

```


> 注意：脚本第一行是自己电脑的Python环境，不要填错。


---


搞定，然后就可以再Query页面下使用快捷键来对代码进行格式化了。
