---
title: 'DCGAN_tf2'
tags: 
- DCGAN
categories: 
- 机器学习
date: "2020-03-15T15:30:34+08:00"
comment:  true  
---

跟着tf官网过一遍DCGAN的算法流程，[源地址](https://www.tensorflow.org/tutorials/generative/dcgan)

<!--more--->

### 导入包

```python
from __future__ import absolute_import, division, print_function, unicode_literals
```

- **absolute_import模块**：一般情况下，程序默认是先在系统的默认目录中寻找导入的包，但是如果我们不想要导入当前目录中的包，就可以用__future__ import absolute_import，这样就可以标准的包
- **divisio**n:在python2和python3中的/除号不同，3中/默认是精确除，//才是地板除。如果要在python2中使用3中的除法，就可以通过__future__导入division实现
- **print_function模块**：使所有的print方法都需要加括号，而不是想python2中的那样可以直接写入
- **unicode_literal:**在Python中有些库的接口要求参数必须是str类型字符串，有些接口要求参数必须是unicode类型字符串。对于str类型的字符串，调用len()和遍历时，其实都是以字节为单位的，这个太就比较坑了，同一个字符使用不同的编码格式，长度往往是不同的。对unicode类型的字符串调用len()和遍历才是以字符为单位，这是我们所想要的。另外，Django，Django REST framework的接口都是返回unicode类型的字符串。为了统一，我个人建议使用from __future__ import unicode_literals，将模块中显式出现的所有字符串转为unicode类型
- [参考知乎点滴学习](https://zhuanlan.zhihu.com/p/32756176)

```python
try:
  # %tensorflow_version 只在 Colab 中使用。
  %tensorflow_version 2.x
except Exception:
  pass

import tensorflow as tf
```

- 

```python
!pip install -q image.io  
 #用于生成gif图片 
```

> image.io是读取图片的库
>
> ```python
> >>> import imageio
> >>> im = imageio.imread('imageio:astronaut.png')
> >>> im.shape  # im is a numpy array
> (512, 512, 3)
> >>> imageio.imwrite('astronaut-gray.jpg', im[:, :, 0])
> ```
>
> 

```python
import glob
import imageio
import matplotlib.pyplot as plt
import numpy as np
import os
import PIL
from tensorflow.keras import layers
import time

from IPython import display
```

- **glob** ：`glob.glob`(*pathname*, ***, *recursive(递归)=False*)  返回pathname下符合过滤条件的文件的文件名list

  - ```python
    glob.glob('**/*.txt', recursive=True)
    ['2.txt', 'sub/3.txt']
    ```

- **image**: 上面说了，读取图片用的

- **matplotlib.pyplot**: 包含一系列类似 MATLAB 中绘图函数的相关函数

- **numpy:** 数学方法库

- **os**

- **PIL:**图像处理标准库(例子，模糊图片)

  - ``` 
    from PIL import Image, ImageFilter
    
    # 打开一个jpg图像文件，注意是当前路径:
    im = Image.open('test.jpg')
    # 应用模糊滤镜:
    im2 = im.filter(ImageFilter.BLUR)
    im2.save('blur.jpg', 'jpeg')
    ```

- **tensorflow.keras:**tf2推荐使用的网络搭建框架
- **time**
- **IPython.display**
  - Ipython是python的交互式shell,比默认的pythonshell 好用。
  - display显示文件喽

### 加载数据集

```python
(train_images, train_labels), (_, _) = tf.keras.datasets.mnist.load_data()
train_images = train_images.reshape(train_images.shape[0], 28, 28, 1).astype('float32')
train_images = (train_images - 127.5) / 127.5 # 将图片标准化到 [-1, 1] 区间内
BUFFER_SIZE = 60000
BATCH_SIZE = 256
# 批量化和打乱数据
train_dataset = tf.data.Dataset.from_tensor_slices(train_images).shuffle(BUFFER_SIZE).batch(BATCH_SIZE)
```

- **tf.keras.datasets.mnist.load_data:** 下载数据并分割，返回值`(x_train, y_train), (x_test, y_test)`

- **train_images.reshape()**: 给定张量后，此操作将返回一个新的tf.Tensor，该张量具有与张量相同的值，并且顺序相同，不同之处在于用shape赋予了新的形状

  - ```
    tf.reshape(
        tensor, shape, name=None
    )
    ```

### 创建模型：生成器

```python
def make_generator_model():
    model = tf.keras.Sequential()
    model.add(layers.Dense(7*7*256, use_bias=False, input_shape=(100,)))
    
   	#归一化
    model.add(layers.BatchNormalization())
    #泄漏的整流线性单元
    model.add(layers.LeakyReLU())   
	#整形成7,7,256
    model.add(layers.Reshape((7, 7, 256)))
    assert model.output_shape == (None, 7, 7, 256) # 注意：batch size 没有限制 
	#conv2DTranspose(filters,kernel_size,strides,padding,)参数：输出空间的维度，1个或2个整数表示的元组或者列表指明2D卷积窗口的高度和宽度，strides：1个或2个整数表示的元组或者列表指明卷积沿高度和宽度方向的步长，padding,user_bias是否设置偏置向量。输出张量
    model.add(layers.Conv2DTranspose(128, (5, 5), strides=(1, 1), padding='same', use_bias=False))
    assert model.output_shape == (None, 7, 7, 128)
    
    #重复1
    model.add(layers.BatchNormalization())
    model.add(layers.LeakyReLU())
    model.add(layers.Conv2DTranspose(64, (5, 5), strides=(2, 2), padding='same', use_bias=False))
    assert model.output_shape == (None, 14, 14, 64)
    
    #重复2
    model.add(layers.BatchNormalization())
    model.add(layers.LeakyReLU())
    model.add(layers.Conv2DTranspose(1, (5, 5), strides=(2, 2), padding='same', use_bias=False, activation='tanh'))
    assert model.output_shape == (None, 28, 28, 1)

    return model
```

- tf.keras.layers.LeakyReLU: 泄漏的整流线性单元，当神经元未激活时，它仍允许赋予一个很小的梯度： `f(x) = alpha * x for x < 0`, `f(x) = x for x >= 0`.

  - ```python
    tf.keras.layers.LeakyReLU(
        alpha=0.3, **kwargs
    )
    ```

    :grey_question: 不是很懂。。。

- layers.Conv2DTranspose： 转置卷积层，

  #### 测试：用生成器生成一张图片

  ```python
  generator = make_generator_model()
  
  noise = tf.random.normal([1, 100])
  generated_image = generator(noise, training=False)
  
  plt.imshow(generated_image[0, :, :, 0], cmap='gray')
  ```

  - 把噪声输入生成器就可以了。

### 创建模型：判别器

```python
def make_discriminator_model():
    model = tf.keras.Sequential()
    model.add(layers.Conv2D(64, (5, 5), strides=(2, 2), padding='same',
                                     input_shape=[28, 28, 1]))
    model.add(layers.LeakyReLU())
    model.add(layers.Dropout(0.3))

    model.add(layers.Conv2D(128, (5, 5), strides=(2, 2), padding='same'))
    model.add(layers.LeakyReLU())
    model.add(layers.Dropout(0.3))

    model.add(layers.Flatten())
    model.add(layers.Dense(1))

    return model
```

- 判别器是一个基于CNN的图片分类器

- **layers.Dropout(0.3)**: keras.layers.Dropout(rate, noise_shape=**None**, seed=**None**)

  - 为了防止过拟合，在每次训练中随机丢弃一些数据。rate:丢弃的比例

    

  #### 测试：用判别器对图片的真伪进行判断。

  ```python
  discriminator = make_discriminator_model()
  decision = discriminator(generated_image)
  print (decision)
  ```

### 定义损失函数和优化器

```python
# 该方法返回计算交叉熵损失的辅助函数
cross_entropy = tf.keras.losses.BinaryCrossentropy(from_logits=True)
# 判别器损失
def discriminator_loss(real_output, fake_output):
    real_loss = cross_entropy(tf.ones_like(real_output), real_output)
    fake_loss = cross_entropy(tf.zeros_like(fake_output), fake_output)
    total_loss = real_loss + fake_loss
    return total_loss
# 生成器损失
def generator_loss(fake_output):
    return cross_entropy(tf.ones_like(fake_output), fake_output)
```

- **判别器损失：**该方法量化判别器从判断真伪图片的能力。它将判别器对真实图片的预测值与值全为 1 的数组进行对比，将判别器对伪造（生成的）图片的预测值与值全为 0 的数组进行对比。

- **生成器损失：**生成器损失量化其欺骗判别器的能力。直观来讲，如果生成器表现良好，判别器将会把伪造图片判断为真实图片（或 1）。这里我们将把判别器在生成图片上的判断结果与一个值全为 1 的数组进行对比。

- 

- tf.ones_like():实例化与另一个张量相同形状的全一变量。

  - keras.backend.ones_like(x, dtype=**None**, name=**None**)

  - x：张量或者变量

  - dtype:字符串，返回keras变量的类型。如果为 None，则使用 x 的类型。

  - ```python
    >>> from keras import backend as K
    >>> kvar = K.variable(np.random.random((2,3)))
    >>> kvar_ones = K.ones_like(kvar)
    >>> K.eval(kvar_ones)
    array([[ 1.,  1.,  1.],
           [ 1.,  1.,  1.]], dtype=float32)
    ```

  - 

- tf.zeros_like():实例化与另一个张量相同尺寸的全零变量。参数和上面相同只不过形状相同，用0填充

```python
#分别定义优化器
generator_optimizer = tf.keras.optimizers.Adam(1e-4)
discriminator_optimizer = tf.keras.optimizers.Adam(1e-4)
```

### 保存检查点：

```python
checkpoint_dir = './training_checkpoints'
checkpoint_prefix = os.path.join(checkpoint_dir, "ckpt")
checkpoint = tf.train.Checkpoint(generator_optimizer=generator_optimizer,
                                 discriminator_optimizer=discriminator_optimizer,
                                 generator=generator,
                                 discriminator=discriminator)
```

对任务中断再恢复有帮助。

### 定义训练循环

训练循环在生成器接收到一个随机种子作为输入时开始。该种子用于生产一张图片。判别器随后被用于区分真实图片（选自训练集）和伪造图片（由生成器生成）。针对这里的每一个模型都计算损失函数，并且计算梯度用于更新生成器与判别器

```python
EPOCHS = 50
noise_dim = 100
num_examples_to_generate = 16

# 我们将重复使用该种子（因此在动画 GIF 中更容易可视化进度）
seed = tf.random.normal([num_examples_to_generate, noise_dim])


# 注意 `tf.function` 的使用
# 该注解使函数被“编译”
@tf.function
def train_step(images):
    noise = tf.random.normal([BATCH_SIZE, noise_dim])

    with tf.GradientTape() as gen_tape, tf.GradientTape() as disc_tape:
      generated_images = generator(noise, training=True)

      real_output = discriminator(images, training=True)
      fake_output = discriminator(generated_images, training=True)

      gen_loss = generator_loss(fake_output)
      disc_loss = discriminator_loss(real_output, fake_output)

    gradients_of_generator = gen_tape.gradient(gen_loss, generator.trainable_variables)
    gradients_of_discriminator = disc_tape.gradient(disc_loss, discriminator.trainable_variables)

    generator_optimizer.apply_gradients(zip(gradients_of_generator, generator.trainable_variables))         discriminator_optimizer.apply_gradients(zip(gradients_of_discriminator,
  																  discriminator.trainable_variables))
    
def train(dataset, epochs):
  for epoch in range(epochs):
    start = time.time()

    for image_batch in dataset:
      train_step(image_batch)

    # 继续进行时为 GIF 生成图像
    display.clear_output(wait=True)
    generate_and_save_images(generator,
                             epoch + 1,
                             seed)

    # 每 15 个 epoch 保存一次模型
    if (epoch + 1) % 15 == 0:
      checkpoint.save(file_prefix = checkpoint_prefix)

    print ('Time for epoch {} is {} sec'.format(epoch + 1, time.time()-start))

  # 最后一个 epoch 结束后生成图片
  display.clear_output(wait=True)
  generate_and_save_images(generator,epochs,seed)
```

### 生成保存图片：

```python
def generate_and_save_images(model, epoch, test_input):
  # 注意 training` 设定为 False
  # 因此，所有层都在推理模式下运行（batchnorm）。
  predictions = model(test_input, training=False)

  fig = plt.figure(figsize=(4,4))

  for i in range(predictions.shape[0]):
      plt.subplot(4, 4, i+1)
      plt.imshow(predictions[i, :, :, 0] * 127.5 + 127.5, cmap='gray')
      plt.axis('off')

  plt.savefig('image_at_epoch_{:04d}.png'.format(epoch))
  plt.show()
```

### 正式训练：

```python
%%time
train(train_dataset, EPOCHS)
```

### 其他

#### 	恢复最新的检查点：

```python
checkpoint.restore(tf.train.latest_checkpoint(checkpoint_dir))
```

####      创建gif:

```python
# 使用 epoch 数生成单张图片
def display_image(epoch_no):
  return PIL.Image.open('image_at_epoch_{:04d}.png'.format(epoch_no))
display_image(EPOCHS)
```

​		使用训练过程中生成的图片通过imagesio生成gif

```python
anim_file = 'dcgan.gif'

with imageio.get_writer(anim_file, mode='I') as writer:
  filenames = glob.glob('image*.png')
  filenames = sorted(filenames)
  last = -1
  for i,filename in enumerate(filenames):
    frame = 2*(i**0.5)
    if round(frame) > round(last):
      last = frame
    else:
      continue
    image = imageio.imread(filename)
    writer.append_data(image)
  image = imageio.imread(filename)
  writer.append_data(image)

import IPython
if IPython.version_info > (6,2,0,''):
  display.Image(filename=anim_file)
```

