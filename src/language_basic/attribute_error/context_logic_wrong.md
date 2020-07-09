# 上下代码逻辑问题

此处介绍，上下代码逻辑问题导致，获取到的值xxx，不是希望的值，导致属性报错的情况。

## 举例：调用函数之前多了个=等于号

### 问题

[制作数据集时报错AttributeError: 'str' object has no attribute 'write'-CSDN论坛](https://bbs.csdn.net/topics/395845149)

### 解答

基本上确定了，就是其自己笔误：多写了个等于号

把：

```python
writer= tf.compat.v1.python_io.TFRecordWriter("mask_and_nomask_test.tfrecords")
```

写成：

```python
writer= tf.compat.v1.python_io.TFRecordWriter=("mask_and_nomask_test.tfrecords")
```

导致此处的`writer`：

* 不是：原本希望的TFRecordWriter()所返回的变量
* 而是：一个普通的字符串

因为：

```python
writer= tf.compat.v1.python_io.TFRecordWriter=("mask_and_nomask_test.tfrecords")
```

等价于：

```python
tf.compat.v1.python_io.TFRecordWriter = ("mask_and_nomask_test.tfrecords")
writer = ("mask_and_nomask_test.tfrecords")
```

等价于：

```python
tf.compat.v1.python_io.TFRecordWriter = "mask_and_nomask_test.tfrecords"
writer = "mask_and_nomask_test.tfrecords"
```

此时`writer`变量只是个str字符串。所以此处才报错：writer（这个str字符串变量）没有（TFRecordWriter才有的）write这个属性

### 解决办法

去掉你的笔误，即去掉多写的那个**等于号**`=`

```python
writer= tf.compat.v1.python_io.TFRecordWriter("mask_and_nomask_test.tfrecords")
```


其含义是：

* [tf.data.experimental.TFRecordWriter  |  TensorFlow Core v2.1.0](https://www.tensorflow.org/api_docs/python/tf/data/experimental/TFRecordWriter)
* [tf.io.TFRecordWriter  |  TensorFlow Core v2.1.0](https://www.tensorflow.org/api_docs/python/tf/io/TFRecordWriter)

调用了：

* 函数：`tf.compat.v1.python_io.TFRecordWriter`
  * 传入的参数是：`"mask_and_nomask_test.tfrecords"`
  * 才会返回
    * 对应的类`TFRecordWriter`
      * 其才有`write`函数
        * 后续的
          * `writer.write(xxx)`
          * 才能正常运行。
