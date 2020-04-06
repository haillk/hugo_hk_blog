---
title: 'Tfrecord'
tags: 
-
categories: 
- 机器学习
date: "2020-03-16T22:59:58+08:00"
comment:  true  
---



### TFrecord文件定义：

> 这是一种将图像数据和标签放在一起的二进制文件，能更好的利用内存，在tensorflow中快速的复制，移动，读取，存储 等等..

本质上和xml和json差不多，通过将多个示例打包进同一个文件内。

首先从原始图片数据中构建出一个TFrecord文件

```python
image_ds = tf.data.Dataset.from_tensor_slices(all_image_paths).map(tf.io.read_file)
tfrec = tf.data.experimental.TFRecordWriter('images.tfrec')
tfrec.write(image_ds)
```

然后构建一个从Tfrecord文件读取的数据集，并格式化

```python
image_ds = tf.data.TFRecordDataset('images.tfrec').map(preprocess_image)
```

之后压缩该数据集和标签数据集得到（图片，标签）对数据集

```python
ds = tf.data.Dataset.zip((image_ds, label_ds))
ds = ds.apply(
  tf.data.experimental.shuffle_and_repeat(buffer_size=image_count))
ds=ds.batch(BATCH_SIZE).prefetch(AUTOTUNE)
ds
```

