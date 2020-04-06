---
title: 'Tf2_用tfdata加载数据'
tags: 
-
categories: 
- 机器学习
date: "2020-03-16T17:02:36+08:00"
comment:  true  
---



使用tf.data进行图像预处理

<!--more-->

### 配置包

```python
from __future__ import absolute_import, division, print_function, unicode_literals

import tensorflow as tf

AUTOTUNE = tf.data.experimental.AUTOTUNE
```

- 详见DCGAN的那篇文章
- :grey_question: 最后一行还不知道什么意思

### 下载并检查数据集，根目录存在data_root中

#### 检索图片：

​			首先我们有一个储存这原始文件的文件夹。之后导入

```python
import pathlib
data_root_orig = tf.keras.utils.get_file(origin='https://storage.googleapis.com/download.tensorflow.org/example_images/flower_photos.tgz',
                                         fname='flower_photos', untar=True)
data_root = pathlib.Path(data_root_orig)
print(data_root)
```

- tf.keras.utils.get_file： 下载文件喽

- data_root = pathlib.Path(data_root_orig)： 获取根目录

  

```python
import random
all_image_paths = list(data_root.glob('*/*'))
all_image_paths = [str(path) for path in all_image_paths]
random.shuffle(all_image_paths)

image_count = len(all_image_paths)
```

- 这步将根目录下的文件的文件名都存到一个list中，并打乱顺序

#### 检查图片：文件名存在all_image_paths中

#### 确定每张图片用的标签：标签存在all_image_labels中

```python
label_names = sorted(item.name for item in data_root.glob('*/') if item.is_dir())
label_names
##out
['daisy', 'dandelion', 'roses', 'sunflowers', 'tulips']
```

- 上面这里只是将子文件名当作标签输出了

```python
label_to_index = dict((name, index) for index, name in enumerate(label_names))
all_image_labels = [label_to_index[pathlib.Path(path).parent.name]
                    for path in all_image_paths]

print("First 10 labels indices: ", all_image_labels[:10])
##out
First 10 labels indices:  [1, 2, 3, 4, 2, 2, 2, 2, 2, 2]
```

- 然后为标签指定索引

### 加载和格式化图片

```python
#从list中取一个文件名
img_path = all_image_paths[0]
#根据文件名读取文件，原始数据
img_raw = tf.io.read_file(img_path)
print(repr(img_raw)[:100]+"...")
```

- repr(object)：将对象转化为供解释器读取的形式，返回对象的string.最后是返回文件数据的最后一百个字符

````python
img_tensor = tf.image.decode_image(img_raw)

print(img_tensor.shape)
print(img_tensor.dtype)
#out
(266, 320, 3)
<dtype: 'uint8'>
````

- 解码为张量：

```python
img_final = tf.image.resize(img_tensor, [192, 192])
img_final = img_final/255.0
print(img_final.shape)
print(img_final.numpy().min())
print(img_final.numpy().max())
#out
(192, 192, 3)
0.0
0.99897873
```

- 根据模型调整大小，和归一化

```python
def preprocess_image(image):
  image = tf.image.decode_jpeg(image, channels=3)
  image = tf.image.resize(image, [192, 192])
  image /= 255.0  # normalize to [0,1] range

  return image
def load_and_preprocess_image(path):
  image = tf.io.read_file(path)
  return preprocess_image(image)

##test
import matplotlib.pyplot as plt

image_path = all_image_paths[0]
label = all_image_labels[0]

plt.imshow(load_and_preprocess_image(img_path))
plt.grid(False)
plt.xlabel(caption_image(img_path))
plt.title(label_names[label].title())
print()
```

- 最后包装一下

## 构建一个tf.data.Dataset

>  构建tf.data.dataset的最简单方法就是使用from_tensor_slices方法

```python
path_ds = tf.data.Dataset.from_tensor_slices(all_image_paths)
```

- 先对之前的文件名列表切片，得到一个字符串数据集，每个数据项都用（shapes,types）来描述。

````python
image_ds = path_ds.map(load_and_preprocess_image, num_parallel_calls=AUTOTUNE)
````

- 创建一个新的数据集，通过在路径数据集（上面的path_ds）上映射`preprocess_image`（就是上面的）来格式化图片

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(8,8))
for n, image in enumerate(image_ds.take(4)):
  plt.subplot(2,2,n+1)
  plt.imshow(image)
  plt.grid(False)
  plt.xticks([])
  plt.yticks([])
  plt.xlabel(caption_image(all_image_paths[n]))
  plt.show()
```

- 测试验证一下

### 一个（图片，标签）对数据集

```python
label_ds = tf.data.Dataset.from_tensor_slices(tf.cast(all_image_labels, tf.int64))
for label in label_ds.take(10):
  print(label_names[label.numpy()])
```

- 同样使用from_tensor_slices方法，创建一个标签数据集
- :grey_question: label_names[label.numpy()]

```python
image_label_ds = tf.data.Dataset.zip((image_ds, label_ds))
print(image_label_ds)
#out
<ZipDataset shapes: ((192, 192, 3), ()), types: (tf.float32, tf.int64)>
```

- 由于数据集顺序相同，可以把图片集和标签集打包，新的数据集同样是shape+type

### 基本训练方法

要使用此数据集训练模型，你将会想要数据：

- 被充分打乱。

- 被分割为 batch。

- 永远重复。

- 尽快提供 batch。

  

```python
BATCH_SIZE = 32

# 设置一个和数据集大小一致的 shuffle buffer size（随机缓冲区大小）以保证数据
# 被充分打乱。
ds = image_label_ds.shuffle(buffer_size=image_count)
ds = ds.repeat()
ds = ds.batch(BATCH_SIZE)
# 当模型在训练的时候，`prefetch` 使数据集在后台取得 batch。
ds = ds.prefetch(buffer_size=AUTOTUNE)
ds
```

- repeat():显而易见就是重复：没有参数就是无限重复

  - ```python
    dataset = tf.data.Dataset.from_tensor_slices([1, 2, 3]) 
    dataset = dataset.repeat(3) 
    list(dataset.as_numpy_iterator()) 
    #out
    [1,2,3,1,2,3,1,2,3]
    
    ```

- 顺序很重要

### 传递数据集到模型：

为了测试，我们找一个内置的模型副本来feed我们的数据。

```python
mobile_net = tf.keras.applications.MobileNetV2(input_shape=(192, 192, 3), include_top=False)
mobile_net.trainable=False
```

- 这个模型期望他的输入为【-1,1】，所以我们要将我们的数据格式化到【-1,1】

```python
def change_range(image,label):
  return 2*image-1, label

keras_ds = ds.map(change_range)
```

- ds.map():某种映射吧:grey_question:

```python
# 数据集可能需要几秒来启动，因为要填满其随机缓冲区。
image_batch, label_batch = next(iter(keras_ds))

feature_map_batch = mobile_net(image_batch)
print(feature_map_batch.shape)
```

- 这就是就feed数据啦
- MobileNet为每张图片的特征返回一个6*6的网格
- 步骤1：从kera_ds(处理好的数据集)获取batch大小的image和label
- 步骤2：feed到模型

```python
model = tf.keras.Sequential([
  mobile_net,
  tf.keras.layers.GlobalAveragePooling2D(),
  tf.keras.layers.Dense(len(label_names), activation = 'softmax')])
```

- 封装一下，正式构建网络并插入一个[`tf.keras.layers.GlobalAveragePooling2D`](https://www.tensorflow.org/api_docs/python/tf/keras/layers/GlobalAveragePooling2D) 来平均那些空间向量

```python
logit_batch = model(image_batch).numpy()

print("min logit:", logit_batch.min())
print("max logit:", logit_batch.max())
print()

print("Shape:", logit_batch.shape)

#out
min logit: 0.004745514
max logit: 0.738402

Shape: (32, 5)
```

- model().numpy(): .numpy() 方法将张量显示转换为Numpy ndarray

  - ```python
    import numpy as np
    
    ##新建一个nparray
    ndarray = np.ones([3, 3])
    
    ##将nparray转换成张量
    tensor = tf.multiply(ndarray, 42)
    print(tensor)
    
    ##numpy进行操作的时候会自动的将张量钻换位nparray
    print(np.add(tensor, 1))
    
    ##.numpy(）方法明确的将张量转换成numpy
    print(tensor.numpy())
    ```

- 正式构建好model后，再次feed数据

```python
model.compile(optimizer=tf.keras.optimizers.Adam(),
              loss='sparse_categorical_crossentropy',
              metrics=["accuracy"])
```

- 编译模型并描述训练过程，优化器，指标，损失函数等等。

```python
len(model.trainable_variables)
```

- 此处有两个可训练的变量，使用len(m.t_v)可以获取可训练变量的数量。

```python
model.summary()
```

- 这个方法输出模型信息。

```python
steps_per_epoch=tf.math.ceil(len(all_image_paths)/BATCH_SIZE).numpy()
steps_per_epoch
```

- 指定epoch

```python
model.fit(ds, epochs=1, steps_per_epoch=3)
```

- 正式训练