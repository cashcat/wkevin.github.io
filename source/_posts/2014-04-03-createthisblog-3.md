title: 本博客的搭建（续2）
date: 2014-04-03 15:47:57
tags: 
- hexo
- github
- blog
---

## 关于 .deploy
.deploy/ 在 `hexo init` 的时候是没有的，只是在 `hexo deploy`才创建，基本流程我猜测是：
*	根据 _config.yml 中指定的 repo和branch，创建 .deploy/
*	git clone
*	将 public/ 合入 .deploy/ 
*	git push .deploy/

**有两种不同的处理方式**

### （一）submodule 管理 .deploy/

	git submodule add https://github.com/kevinwjj/kevinwjj.github.io.git .deploy/

使用时

	git clone https://github.com/kevinwjj/kevinwjj.github.io.git
	cd kevinwjj.github.io
	git submodule init  
	git submodule update 

init和update不指定的话，会初始化和clone所有submodule：iOS7、landscape、light、.deploy

**这样的话，.deploy其实又指向了自己，变成递归了，My god！—— 这个坑好像不小啊** 

坑实在太深，`hexo d`的时候常常异常，需要对submodule的使用理解深入，不得已想下面的办法。

### （二）.gitignore 管理 .deploy/

首先是加入到 .gitignore 中：

	rm -rf .deploy/
	git rm -rf .deploy
	echo .deploy >> .gitignore
	git commit
	git push origin write:write

然后是在另一台电脑上即可进行clone及其使用

	git clone https://github.com/kevinwjj/kevinwjj.github.io.git
	git checkout -b write origin/write
				//此时 .deploy 是空的
	hexo g 		//生成了public/
	hexo d 		//生成了.deploy/


**问题是**，`hexo d`之后，html确实是退到github的master分支了，但**master分支会被重置**

	$ git logk origin/master
 	 5c14c33 2014-04-03 16:19:02 +0800  kevinWJJ  Site updated: 2014-04-03 16:19:02
 	 edf0f22 2014-04-03 16:19:01 +0800  kevinWJJ  First commit

以前提交的master分支都完蛋了，这是个很严重的问题啊。这可咋整？曲线救国吧。写个脚本自己来deploy：

	wangkevintekiMacBook-Pro:write wangkevin$ cat deployByMyself 
	#!/bin/bash
	hexo g
	cp -R public/. ../kevinwjj.github.io/
	cd ../kevinwjj.github.io/
	git add .
	git commit -m "Deploy by kevinWJJ from hexo"
	git push origin master:master
	cd -

思路就是：public中生成html后，手工拷贝到另外一个master分支的repo中，提交之，然后回来fetch一下，再把writeblog也提交了。
**手工提交，心里舒服多了。**

## 关于 .gitignore

使用`hexo init`新建一个 hexo “工厂”（暂时成为工厂吧，我感觉类似与生产 html格式blog的工厂）后，自动生成的 .gitignore 包含

	$ cat .gitignore
	.DS_Store		//是OS X系统使用的管理文件
	node_modules	//存放plugin的
	db.json			//虽然不太明确，看样子是保存数据，做数据库用的

根据上面**hexo默认忽略的文件**修改出**我的 .gitignore**：

	.DS_Store
	db.json
	public 			//每次可以自动生成，属于过程文件
	.deploy 

## 其他
*	`hexo new "***"` \*\*\*不需要加 .md，因为 _config.yml 中配置了自动加 .md 后缀
	`new_post_name: :title.md # File name of new posts`
可以修改为 `new_post_name: :year-:month-:day-:title.md`，使其自动添加年月日，blog写多了，排排序总是好的，想的有点远，呵呵，说不定写不了几篇就罢笔了呢。
*	有3种blog：post（默认）、page、draft