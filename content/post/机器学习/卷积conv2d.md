---
title: '卷积conv2d'
tags: 
-
categories: 
- 机器学习
date: "2020-03-20T23:18:48+08:00"
comment:  true  
---

### Conv2d

[conv2d参考,看这个足够了](https://blog.csdn.net/lujiandong1/article/details/53728053)

[参考原文](https://stackoverflow.com/questions/37674306/what-is-the-difference-between-same-and-valid-padding-in-tf-nn-max-pool-of-t)

函数定义：

```python
keras.layers.Conv2D(filters, kernel_size, strides=(1, 1), padding='valid', data_format=None, dilation_rate=(1, 1), activation=None, use_bias=True, kernel_initializer='glorot_uniform', bias_initializer='zeros', kernel_regularizer=None, bias_regularizer=None, activity_regularizer=None, kernel_constraint=None, bias_constraint=None)

```

- 该层创建一个卷积核，对层输入惊醒卷积以生成张量。如果user_bias为true，则回创建一个偏置向量添加到输出中。并应用activation.

参数：

> - **filters**: 整数，输出空间的维度 （即卷积中滤波器的输出数量）。
> - **kernel_size**: 一个整数，或者 2 个整数表示的元组或列表， 指明 2D 卷积窗口的宽度和高度。 可以是一个整数，为所有空间维度指定相同的值。
> - **strides**: 一个整数，或者 2 个整数表示的元组或列表， 指明卷积沿宽度和高度方向的步长。 可以是一个整数，为所有空间维度指定相同的值。 指定任何 stride 值 != 1 与指定 `dilation_rate` 值 != 1 两者不兼容。
> - **padding**: `"valid"` 或 `"same"` (大小写敏感)。
> - **data_format**: 字符串， `channels_last` (默认) 或 `channels_first` 之一，表示输入中维度的顺序。 `channels_last` 对应输入尺寸为 `(batch, height, width, channels)`， `channels_first` 对应输入尺寸为 `(batch, channels, height, width)`。 它默认为从 Keras 配置文件 `~/.keras/keras.json` 中 找到的 `image_data_format` 值。 如果你从未设置它，将使用 `channels_last`。
> - **dilation_rate**: 一个整数或 2 个整数的元组或列表， 指定膨胀卷积的膨胀率。 可以是一个整数，为所有空间维度指定相同的值。 当前，指定任何 `dilation_rate` 值 != 1 与 指定 stride 值 != 1 两者不兼容。
> - **activation**: 要使用的激活函数 (详见 [activations](https://keras.io/zh/activations/))。 如果你不指定，则不使用激活函数 (即线性激活： `a(x) = x`)。
> - **use_bias**: 布尔值，该层是否使用偏置向量。
> - **kernel_initializer**: `kernel` 权值矩阵的初始化器 (详见 [initializers](https://keras.io/zh/initializers/))。
> - **bias_initializer**: 偏置向量的初始化器 (详见 [initializers](https://keras.io/zh/initializers/))。
> - **kernel_regularizer**: 运用到 `kernel` 权值矩阵的正则化函数 (详见 [regularizer](https://keras.io/zh/regularizers/))。
> - **bias_regularizer**: 运用到偏置向量的正则化函数 (详见 [regularizer](https://keras.io/zh/regularizers/))。
> - **activity_regularizer**: 运用到层输出（它的激活值）的正则化函数 (详见 [regularizer](https://keras.io/zh/regularizers/))。
> - **kernel_constraint**: 运用到 `kernel` 权值矩阵的约束函数 (详见 [constraints](https://keras.io/zh/constraints/))。
> - **bias_constraint**: 运用到偏置向量的约束函数 (详见 [constraints](https://keras.io/zh/constraints/))



### Conv2DTranspose

函数定义

```python
keras.layers.Conv2DTranspose(filters, kernel_size, strides=(1, 1), padding='valid', output_padding=None, data_format=None, dilation_rate=(1, 1), activation=None, use_bias=True, kernel_initializer='glorot_uniform', bias_initializer='zeros', kernel_regularizer=None, bias_regularizer=None, activity_regularizer=None, kernel_constraint=None, bias_constraint=None)

```

- 转置卷积层
- 将具有卷积输出尺寸的东西转换成具有卷积输入尺寸的东西。

参数

> - **filters**: 整数，输出空间的维度 （即卷积中滤波器的输出数量）。
> - **kernel_size**: 一个整数，或者 2 个整数表示的元组或列表， 指明 2D 卷积窗口的高度和宽度。 可以是一个整数，为所有空间维度指定相同的值。
> - **strides**: 一个整数，或者 2 个整数表示的元组或列表， 指明卷积沿高度和宽度方向的步长。 可以是一个整数，为所有空间维度指定相同的值。 指定任何 stride 值 != 1 与指定 `dilation_rate` 值 != 1 两者不兼容。
> - **padding**: `"valid"` 或 `"same"` (大小写敏感)。
> - **output_padding**: 一个整数，或者 2 个整数表示的元组或列表， 指定沿输出张量的高度和宽度的填充量。 可以是单个整数，以指定所有空间维度的相同值。 沿给定维度的输出填充量必须低于沿同一维度的步长。 如果设置为 `None` (默认), 输出尺寸将自动推理出来。
> - **data_format**: 字符串， `channels_last` (默认) 或 `channels_first` 之一，表示输入中维度的顺序。 `channels_last` 对应输入尺寸为 `(batch, height, width, channels)`， `channels_first` 对应输入尺寸为 `(batch, channels, height, width)`。 它默认为从 Keras 配置文件 `~/.keras/keras.json` 中 找到的 `image_data_format` 值。 如果你从未设置它，将使用 "channels_last"。
> - **dilation_rate**: 一个整数或 2 个整数的元组或列表， 指定膨胀卷积的膨胀率。 可以是一个整数，为所有空间维度指定相同的值。 当前，指定任何 `dilation_rate` 值 != 1 与 指定 stride 值 != 1 两者不兼容。
> - **activation**: 要使用的激活函数 (详见 [activations](https://keras.io/zh/activations/))。 如果你不指定，则不使用激活函数 (即线性激活： `a(x) = x`)。
> - **use_bias**: 布尔值，该层是否使用偏置向量。
> - **kernel_initializer**: `kernel` 权值矩阵的初始化器 (详见 [initializers](https://keras.io/zh/initializers/))。
> - **bias_initializer**: 偏置向量的初始化器 (详见 [initializers](https://keras.io/zh/initializers/))。
> - **kernel_regularizer**: 运用到 `kernel` 权值矩阵的正则化函数 (详见 [regularizer](https://keras.io/zh/regularizers/))。
> - **bias_regularizer**: 运用到偏置向量的正则化函数 (详见 [regularizer](https://keras.io/zh/regularizers/))。
> - **activity_regularizer**: 运用到层输出（它的激活值）的正则化函数 (详见 [regularizer](https://keras.io/zh/regularizers/))。
> - **kernel_constraint**: 运用到 `kernel` 权值矩阵的约束函数 (详见 [constraints](https://keras.io/zh/constraints/))。
> - **bias_constraint**: 运用到偏置向量的约束函数 (详见 [constraints](https://keras.io/zh/constraints/))。