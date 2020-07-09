# 函数参数

函数参数相关常见问题，主要分2类：

* 参数个数不匹配
* 参数位置错误

下面举例说明：

## 参数位置错误=参数顺序搞错了

### 举例：Python的pyecharts参数

#### 问题

[关于Python pyecharts 的问题（已经找资料找了半天了）-CSDN论坛](https://bbs.csdn.net/topics/396136267?page=1#post-410978309)

> 
```python
name=['A','B','C','D','E']
values=[1,2,3,4,5]
wordcloud.add("",name,values,word_size_range=[20,100],shape= "circle")
```
>
> 以上程序会抛出
> 
> `TypeError: add() takes 3 positional arguments but 4 positional arguments (and 2 keyword-only arguments) were given`
>
> 难道只能用官网上给的列表嵌套元组的形式吗？但我看到过类似我这样写的。。。。0.0

#### 解释

对于：

```python
wordcloud.add("",name,values,word_size_range=[20,100],shape= "circle")
```

中的`wordcloud`的`add`函数，去找了下，过程如下：

google搜：

`pyecharts add`

找到

[30分钟学会pyecharts数据可视化 - 知乎](https://zhuanlan.zhihu.com/p/63236019)

人家写法是：

```python
cloud.add(name = 'utils',attr = words,value = counts,
          shape = "circle",word_size_range = (10,70))
```

-> 也没有你的

`wordcloud.add("",name`

中的第一个，空字符串：`""`

后来找到官网文档

[Documentation - pyecharts-en](https://pyecharts.readthedocs.io/en/latest/en-us/documentation/#wordcloud)

中是：

> WordCloud
> 
> WordCloud.add() signatures
> 
> add(name, attr, value, shape="circle", word_gap=20, word_size_range=None, rotate_step=45)

-> 更没有你写的 第一个参数 空字符串 `""`

-> 所以结论很明显：

看起来是，你多传递了一个参数，第一个的空字符串 `""`

后来发现：实际上也不是，而是：

对照官网参数顺序：

**name, attr, value**, shape="circle", word_gap=20, word_size_range=None, .....

而你的是：

**"",name,values**,word_size_range=[20,100],shape= "circle"

很明显是（看起来是？）：你把参数的顺序搞错了吧？

（看起来）应该改为：

wordcloud.add(**name,"",values**,word_size_range=[20,100],shape= "circle")

其中的 **""** 对应着第二个参数：`attr`

注：我不是很确定你代码逻辑，需要你自己明确要给

`name, attr, value`

传递具体什么值。

#### 引申=心得

-》不要随便参考别人（可能错误，可能是落后的，没更新的）代码

-》或者自己瞎猜一个（函数的参数，和顺序）

无论如何，都应该是：

* 改为正确的学习思路和方法
    * 核心要点是：去找官网正规资料
      * 关于
        * 如何掌握正确的学习方法和思路
        * 如何利用google查找到自己要的资料
      * 可参考我的：
        * [学习方法思路及技术心得总结](https://crifan.github.io/learn_tech_method_experience/website/)
