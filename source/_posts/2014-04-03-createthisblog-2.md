title: 本博客的搭建（续）
date: 2014-04-03 11:37:24
tags: hexo
---

昨晚弄到凌晨1点多，高兴的碎觉去了。今早到了公司，另选一台电脑，git clone 出来本博客
`git clone htts://github.com/kevinwjj/kevinwjj.github.io.git`

当想在 write 分支开写的时候，发现不行了。

layout错误
---

	$ hexo g
	[info] Files loaded in 0.298s
	[warn] No layout: archives/
	[warn] No layout: archives/2014/
	[warn] No layout: archives/2014/04/
	[warn] No layout:
	[warn] No layout: 2014/04/02/本博客的搭建/
	[warn] No layout: tags/hexo/
	[create] Public: atom.xml
	[info] 1 files generated in 0.209s


分析
---
版本回滚 `git reset --hard HEAD~1` —— 不行。

仔细检查，发现 themes/ 目录下的 landscape、iOS7、light 都是空的。

一番百度，原来是 git submodule 的问题。

速速学习几篇 [git submodule 相关操作解析](http://my.oschina.net/shede333/blog/186626) ，马上开始修改

working tree上操作submodule
---

	$ git rm -rf light iOS7 landscape
	$ git submodule add https://github.com/hexojs/hexo-theme-light.git light
	$ git submodule add https://github.com/tracy-e/hexo-theme-iOS7.git iOS7
	$ git submodule add https://github.com/hexojs/hexo-theme-landscape.git landscape
	$ cat .gitmodules
	[submodule "themes/light"]
        path = themes/light
        url = https://github.com/hexojs/hexo-theme-light.git
	[submodule "themes/landscape"]
        path = themes/landscape
        url = https://github.com/hexojs/hexo-theme-landscape.git
	[submodule "themes/iOS7"]
        path = themes/iOS7
        url = https://github.com/tracy-e/hexo-theme-iOS7.git


提交 submodule
---
OK，本地已经没问题了，submodule的信息也要传到github上啊

	$ git add .
	$ git commit
	$ git push

其实只commit了 .gitmodules 文件，3个theme的文件都没有上传。

再次clone验证submodule
---

	$ git clone https://github.com/kevinwjj/kevinwjj.github.io.git temp
	$ cd temp/themes/
	$ git submodule init iOS7
	$ git submodule update iOS7

我理解，每次`git fetch` 或 `git pull`整个repo的时候，submodule不更新，需要单独执行`git submodule update`。

	为什么要指定 iOS7？
	因为不指定的话我会得到这个error
	No submodule mapping found in .gitmodules for path '.deploy'
	好像 .deploy 也是一个submodule似得，不太明确，先放一下吧。

