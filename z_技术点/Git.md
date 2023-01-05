本地代码 --> 暂存区 --> 本地库 --> 远程库

#上传到GitHub

上传到Gitee
创建仓库
设置成公有
安装Git
设置用户名和邮箱
	1.用户名和邮箱的作用
		用户名和邮箱地址是本地g1t客户端的一个变量·用户每次提交代码都会记录
	2,设置
		2.1设置用户名
			git config --global user.name "username"
		2.2设置邮箱（没有双引号）
			git config --global user.email useremail@qq.com
	3.查看用户名和密码
		git config user.name
		git config user.email
	4·查看其他配置信息(g1t设置列表)
		git config --list
在idea中设置git
选中项目 点击VCS create Git Reponsitory 新建git仓库 项目中的文件会变颜色
右键项目 Git - add 放到暂存区
右键项目 Git - repository 设置远程仓库地址
右键项目 Git -  commit 描述提交信息
右键项目 Git - push
推送成功
