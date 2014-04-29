title: CALayer 和 UIView
date: 2014-04-29 21:19:22
categories: iOS
tags: 
- CALayer
- UIView
---


对于一个CALayer，bounds、position、anchorPoint、frame之间的关系；对于UIView，frame、bounds、center之间的关系；是长久以来网上热议的话题，各种图解、详解一搜大把，但每个人有每个人的理解思路和学习方法，比如我，看来好多篇也不能在脑子中形成完整的理性和感性认识，这可能就是所谓的“智商不够”吧，没办法。

## CALayer
![][calayer_bounds]


下面只能摘录部分，再加上个人理解，拼凑一下了。

首先记住一点：**position 和 bounds 是基础，frame是派生属性。**

*	**bounds** = 左上角相对于自身坐标系的CGPoint（bounds.origin) + 没有坐标系概念的CGSize(bounds.size)
	*	bounds 可以理解为一个遮盖罩
	*	bounds.origin 
		*	默认是(0,0)
		*	不影响层本身的布局，会影响它内部的子层的布局
	*	设置 bounds 可以理解为两步：
		*	用 bounds.origin 设置左上角在本地坐标系的坐标，**左上角不变，反推本地坐标系原点（有点费解啊）**，原点变了会影响subView进行重新布局
		*	用 bounds.size 设置本view/layer的size，以“锚点不动”为前提
*	**poisition** 和 **anchorPoint** 是锚点的“点值表示”和“比例表示”
	*	**position** 是锚点在**父Layer坐标系**的 CGPoint 表示
		*	父Layer坐标系的原点不一定是父Layer的左上角
		*	是锚点的坐标，不是中心点的坐标哦：锚点可以修改的
	*	**anchorPoint = （position - frame.origin）/ frame.size**
		*	anchorPoint 改变，position 随之改变
			*	如： anchorPoint = (0,0),则 position 就移动到左上角
		*	anchorPoint 同时也是变换支点，其实可以理解为 position 是变换支点
		*	操作 position 或 transform 时，anchorPoint 的作用很明显
			*	如：
*	**frame** 的计算参考4个:bounds，anchorPoint，transform，和position
	*	frame 是由 position 和 bounds 计算而得，建议少用
		*	设置frame会反向计算 position 和 bounds，并存储他们两个。
			*	bounds.size = frame.size //bounds原点不变，size变
			*	position 是如何受影响的？
				*	如果是用frame新建layer，position为中心点
				*	如果是修改已有layer的frame，如果 position 不在 center 呢？
	*	设置position：
		*	bounds 根据 anchorPoint 做相应改变？


## UIView

![][uiview_coordinates]

CALayer是UIView的底层实现，view.frame简单的返回了view.layer.frame

UIView 中没有 position 属性，有 center 属性
frame 和 bounds 并非总是相同，一旦旋转，则不相同 

center和bounds属性是相互独立的. 也就是他们中间某一个发生了变化, 不会影响另一个
*	center 决定自己的位置
*	bounds 决定自己的大小，和 subview 的位置

```
frame.origin = center - (bounds.size / 2.0)
center = frame.origin + (bounds.size / 2.0)
frame.size = bounds.size
```

*	when you change the frame, you’re really changing the bounds and center
*	you change the center, you’re updating both the center and the frame.



一个view的显示位置（布局）取决于两点：它在父view的位置，和它相对于center的size
*	定义 frame 可以同时定义这两点，所以可以直接影响view的显示
*	修改 bounds.size 会围绕 center 改变 size，同时，frame 的origin和size也随之而变
*	修改 bounds.origin 不会影响这两点，所以view的显示不变
	*	修改 bounds.origin 会**间接**影响本地坐标系原点，从而影响其subview的位置

更改了bounds.x或者bounds.y,UIView的位置和大小完全不为所动, 但是UIView的所有subView都会平移一段距离(-bounds.x,-bounds.y)。


[CA-class]: https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Animation_Types_Timing/Art/animations_info_2x.png

[calayer_bounds]: http://genekc07.stowers.org/Users/mec/Library/Developer/Shared/Documentation/DocSets/com.apple.adc.documentation.AppleiOS5_1.iOSLibrary.docset/Contents/Resources/Documents/documentation/Cocoa/Conceptual/CoreAnimation_guide/art/layer_bounds.jpg

[uiview_coordinates]: http://i.stack.imgur.com/wUOYV.jpg "frame 和 bounds 并非总是相同，一旦旋转，则不相同"