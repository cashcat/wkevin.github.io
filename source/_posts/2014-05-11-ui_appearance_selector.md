title: UI_APPEARANCE_SELECTOR
date: 2014-05-11 21:10:44
categories: iOS
tags:
---

iOS中改变一个对象的外观的两种方法：  
1.	一次改变此对象（object）的所有实例（instances）  
2.	改变某个 container 中的 instances  

如何做到这两类操作？—— 支持 UIAppearance Protocol。   

**UIAppearance protocol 只包括两个方法**：   

```
+ (instancetype)appearance   
+ (instancetype)appearanceWhenContainedIn:(Class <UIAppearanceContainer>)ContainerClass,...
```

返回值都是获取接受者的 appearance proxy（理解为获取app中所有此class的实例更合适），然后给 proxy 发“**一些**”消息可以批量修改它们的属性。

好了，UIView（UIControl）都支持 UIAppearance Protocol，可以轻松的改变控件的全局样式。 

举例：  
1.	改变所有 UINavigationBar 的 backgroundcolor:   
```
[[UINavigationBar appearance] setBarTintColor:myNavBarBackgroundColor];
```
2.	改变 UINavigationBar 中所有 UIButton 的背景图片：   
```
    [[UIBarButtonItem appearanceWhenContainedIn:[UINavigationBar class], nil]
       setBackgroundImage:myNavBarButtonBackgroundImage forState:state barMetrics:metrics];
```    

那么，哪些方法能够被 appearance proxy 接收呢？ —— **标记了 `UI_APPEARANCE_SELECTOR` 的方法**。
比如：UIBarButtonItem
```
- (void)setTitlePositionAdjustment:(UIOffset)adjustment
   forBarMetrics:(UIBarMetrics)barMetrics UI_APPEARANCE_SELECTOR;
```