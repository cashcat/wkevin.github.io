title: 本博客的搭建
date: 2014-04-02 23:42:18
tags: 
- hexo
- github
- blog
---

github + hexo => blog
===

*	[nodejs.org](http://nodejs.org) 下载、安装node.js
*	[npm](http://npmjs.org)(node package manager，node包管理器）可用
	*	设置npm代理——在公司需要通过代理连外网
		
		`npm config set proxy http://proxy.example.com:8080`
		`npm set proxy http://proxy.example.com:8080`
	*   或者设置windows cmd的代理
		`set HTTP_proxy=http://proxy.example.com:8080`
		`set HTTPS_proxy=https://proxy.example.com:8080`
		
	*	设置npm镜像[cnpm](http://cnpmjs.org)（默认的库服务器会撞墙，比较慢，甚至更新不了）
		
		`npm config set registry http://registry.cnpmjs.org `
		`npm info underscore ` //查看是否配置正确
*	安装hexo 

    *	`sudo npm install -g hexo` //前面代理和镜像设置好了，这里才会快速和顺利
	*	`mkdir myBlog`
	*	`cd myBlog`
	*	`hexo init`
	*	`hexo generate`
	*	`hexo server` //浏览器中可以 http://localhost:4000 查看到了
	
		  	hexo 的命令简写
				hexo g == hexo generate
				hexo d == hexo deploy
				hexo s == hexo server
				hexo n == hexo new			
*	配置 hexo
	*	theme
		*	安装theme [List](https://github.com/tommy351/hexo/wiki/Themes)
	    	`git clone https://github.com/hexojs/hexo-theme-landscape.git themes/landscape`
	    	`git clone https://github.com/hexojs/hexo-theme-light.git themes/light`
	    	必要的话，也要配置git的代理
		*	配置theme： htemes/xxx/_config.yml —— 根据个人喜好修改
	*	plugin
		*	RSS 插件
			
			`sudo npm install hexo-generator-feed`
		*	sitemap 插件
		
			`npm install hexo-generator-sitemap`	
	*	配置myBlog/_config.yml
		*	指定 theme：
		
				 theme: iOS7 //感觉iOS7这个不错，其实是其他的都没试
		*	指定 plugins：
				
				plugins:
				- hexo-generator-feed
				- hexo-generator-sitemap
		*	其他：根据个人喜好修改
*	写文章
	*	`hexo new "my new post"`
	*	进入 source/_posts/ 使用 Markdown 写 md 文件
		*	OS X 上用 Mou
		*	Windows 上用 MarkdownPad
		*	Linux 上用 SublimeText + Markdown Preview
	*	`hexo s` //localhost预览，如果server一直开着的话，能够自动探测到新的md文件
*	部署到github
	*	github 上创建pages
		*	user pages：[http://kevinwjj.github.io](http://kevinwjj.github.io)对应的库 [http://github.com/kevinwjj/kevinwjj.github.io.git](http://github.com/kevinwjj/kevinwjj.github.io.git)
			使用master分支
		*	project pages:使用某个project的gh-pages分支
	*	修改 myBlog/_config.yml

				deploy:
				  type: github
				  repository: https://github.com/kevinwjj/kevinwjj.github.io.git
				  branch: master
	*	`hexo generate`
	*	`hexo deploy` //推入github的库中
*	将markdown文件也放入github
	*	kevinwjj.github.io.git 的 master 分支存放 html + hexo输出文件
	*	kevinwjj.github.io.git 的 write 分支存放 markdown + hexo 编译文件
	
				cd kevinwjj.github.io
				git checkout --orphan write
				cp -aR ../myBlog/. ./
				git add .
				git commit
				git push origin write:write
			


参考文章
---
[Zipperablog](http://zipperary.com/categories/hexo/) ：非常感谢！