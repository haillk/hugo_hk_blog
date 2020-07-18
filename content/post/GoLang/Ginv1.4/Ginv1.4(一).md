---
title: "Ginv1.4(一)"
date: 2019-11-29T00:16:08+08:00
categories:
- Gin
tags:
- gin Router
- gin 参数获取
keywords:
- gin
#thumbnailImage: //example.com/image.jpg  
---
基础形式 ，参数获取
<!--more-->
## Gin基础形式的API示例 ##
```java
func main() {  
	// 使用默认中间件创建一个gin路由器:  
	router := gin.Default()
	// 路由映射
	router.GET("/someGet", getting)
	router.POST("/somePost", posting)
	router.PUT("/somePut", putting)
	router.DELETE("/someDelete", deleting)
	router.PATCH("/somePatch", patching)
	router.HEAD("/someHead", head)
	router.OPTIONS("/someOptions", options)
	// 启动，可设置端口
	router.Run()
	// router.Run(":3000") for a hard coded port
}
```

## 字符串参数 ##
1. **url : www.xxx.com/user?firstname=haillk**  
    获取方式：  
    c.DefaultQuery("firstname","guest") ----->获取并设置默认值  
    c.Query("firstname")----->获取参数
2. 

