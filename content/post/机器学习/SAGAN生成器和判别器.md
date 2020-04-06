---
title: 'SAGAN生成器和判别器'
tags: 
-
categories: 
- 机器学习
date: "2020-03-23T15:30:18+08:00"
comment:  true    
---

*

### SAGAN

​	生成器：

```python
#将一些层封装成块，，注意这里有SN的参与
def block(input,output_channel,training=false):
    x = BatchNormalization(input)
    x = RELU()
    convtr = conv2dT(output_channels,3,2,padding='same')
    x = SN(convtr)
    
    x = BatchNormalization()<-------------------------------------------
    x = RELU()
    convtr = conv2dT(output_channels,3,2,padding='same')
    x = SN(convtr)
    
    convtr = conv2dT(output_channels,3,2,padding='same')
    x_ = SN(convtr)
    return laters.add(x_,x)
def get_generator(num_classer,gf_dim=16,training=false):
    z=input(name = noisy)
    condition_label = imput()
    one-hot-label = onehot
    x = layers.Concatenate(z,one-hot-label)
    x = SN()
    ...
    x = Block()...
    x = Attention_layer()(x)<---------------------*****************************
    x= Block()...
    x = BatchNormalization()
    x = RELU()
    conv = layers.conv2d()
    output = SN(conv)(x)
    return Model()
```

判别器：

```python
def Optimized_block(inpput,output_shape,training=false):
    conv=conv2d(output_shape)
    x = SN(input)
    x = RELU
    con = conv2d()
    x = SN    
    conv=conv2d
    x_ = SN   
    reutrn laters.add(x_,x)
def BLock():
    RELU
    conv2d
    x = SN
    
    RELU
    conv2d
    SN
    
    RELU
    conv2d
    SN
    return layer.add(x_,x)
def get_discrimiator():
    Optimized_block()
    Block()
    Attention_Layer()
    
    Block()
    Block()
    Block()
    Block()
    
    layer.Relu()
    
    SN
    SN
    return model()
```

这样判别器和生成器中都用到了SN和attention

