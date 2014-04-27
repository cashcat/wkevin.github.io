title: CocoaPods 简要步骤
date: 2014-04-10 01:28:54
categories: iOS
tags: CocoaPods
---

---
### 资源
#### 网站
*	[cocoapods.org](http://cocoapods.org)
*	[github src](https://github.com/CocoaPods/CocoaPods)

#### 文章
*	[CocoaPods安装和使用教程--Code4App](http://code4app.com/article/cocoapods-install-usage)
*	[深入理解 CocoaPods--伯乐在线](http://blog.jobbole.com/53365/)

---
### 安装
*	`$ sudo gem update --system` ：更新gem版本  
*	`$ gem sources -a http://ruby.taobao.org/` ：增加gem库，然后可 *	`gem source -l` 查看一下   
*	`$ sudo gem install cocoapods` ：安装cocoapods

---
### 查看cocoapods是否支持某个第三方库（如：ASValueTrackingSlider）
*	`$ pod search ASValueTrackingSlider` ： 搜索pod库中ABC的信息，支持模糊查找、正则查找
*	`$ pod try ASValueTrackingSlider` ：会创建临时文件夹，并下载ABC，并用XCode打开
*	`$ pod list |grep ASValueTrackingSlider` 

---
### 在自己的XCode项目中引入ABC
*	`$ cd myProjcet`
*	`$ vi Podfile`  : [Podfile官方说明](http://guides.cocoapods.org/using/the-podfile.html)

        platform :ios, '7.0'  
	    pod 'ASValueTrackingSlider',       '~> 0.6'   
	ABC 的版本号可由 `pod search ASValueTrackingSlider`查出，并直接拷贝过来
*	`$ pod install`  ：会生成 myProject.xcworkspace、Podfile.lock、Pods/
*	`$ open myProject.xcworkspace` ：以后每次都要从这里打开XCode
*	`$ pod update` ：已经有 xcworkspace等的项目可以用此命令更新

---
### 创建Pod
*	`$ pod spec create Peanut`
*	`$ edit Peanut.podspec`
*	`$ pod spec lint Peanut.podspec`
