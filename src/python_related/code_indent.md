# 代码缩进问题

## 代码缩进问题，导致代码运行了，以为没运行

### 问题

[求助！！！正则表达式是正确的，但是程序无法运行-CSDN论坛](https://bbs.csdn.net/topics/396092185)

```python
import requests
import json
import re
import time
from requests.exceptions import RequestException
url ='http://www.24timemap.com/'
def get_one_page(url):
    try:
        headers={
            'User-Agent':'Mozilla/5.0(Macintosh;Intel Mac OS X 10_13_3)AppleWebKit/537.36(KHTML,like Gecko) Chrome/65.0.3325.162 Safari/537.36'
        }
        response = requests.get(url,headers=headers)
        if response.status_code ==200:
            return response.text
        return None
    except RequestException:
        return None
def parse_one_page(html):
    pattern = re.compile('<li.*?bg.*?title.*?>(.*?)</a>(.*?)</li>',re.S)
    items = re.findall(pattern,html)
    for item in items:
        yield {
            'locantion': item[0],
            'time': item[1]
        }


def write_to_file(content):
        with open('slw.txt', 'a', encoding='utf-8')as f:
            f.write(json.dumps(content, ensure_ascii=False) + '\n')
```

### 解答

**简答**：不懂代码缩进问题，以为代码没运行，其实正常运行了

**详解**

> 正则表达式是正确的，但是程序无法运行

的确，你的 代码 中  正则（或许）是正确的

而你说的 无法运行，其实：已经运行了

-》已经按照你给的Python的代码 运行了

-》运行结果就是：啥都没干

-》为何啥都没干，是因为你没写代码让其干活

-》具体说就是：你只定义了3个函数（其中一个函数中有你说的正则），但是却没调用（其中任何一个函数）

所以变成你说的：`代码无法运行`

如何让其运行？

比如代码最后加上

```python
pageHtml = get_one_page(url)
respGenerator = parse_one_page(pageHtml)
for eachItemDict in respGenerator:
  write_to_file(eachItemDict)
```

就可以实现：

真正调用函数 执行内部的代码和逻辑 -》变成你说希望的：代码真的运行了。

另外，关于代码缩进 导致你代码运行了 但你以为没运行，详见：

* [【教程】详解Python中代码缩进（Indent）：影响代码的内在逻辑关系和执行结果 – 在路上](https://www.crifan.com/tutorial_python_indent/)
* [5.4. Python中的缩进 - python初级教程：入门详解](https://www.crifan.com/files/doc/docbook/python_beginner_tutorial/release/html/python_beginner_tutorial.html#python_indent)