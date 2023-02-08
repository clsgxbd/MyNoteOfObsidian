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
右键项目 Git - add 放到暂存区  `取消: git reset`
右键项目 Git - repository 设置远程仓库地址
右键项目 Git -  commit 描述提交信息 `取消: git reset --soft HEAD^`
右键项目 Git - push
推送成功


#### git stash 
参考链接: https://www.cnblogs.com/zndxall/archive/2018/09/04/9586088.html
```
常用git stash命令：
（1）**git stash** save "save message"  : 执行存储时，添加备注，方便查找，只有git stash 也要可以的，但查找时不方便识别。
（2）**git stash list**  ：查看stash了哪些存储
（3）**git stash show** ：显示做了哪些改动，默认show第一个存储,如果要显示其他存贮，后面加stash@{$num}，比如第二个 git stash show stash@{1}
（4）**git stash show -p** : 显示第一个存储的改动，如果想显示其他存存储，命令：git stash show  stash@{$num}  -p ，比如第二个：git stash show  stash@{1}  -p
（5）**git stash apply** :应用某个存储,但不会把存储从存储列表中删除，默认使用第一个存储,即stash@{0}，如果要使用其他个，git stash apply stash@{$num} ， 比如第二个：git stash apply stash@{1} 
（6）**git stash pop** ：命令恢复之前缓存的工作目录，将缓存堆栈中的对应stash删除，并将对应修改应用到当前的工作目录下,默认为第一个stash,即stash@{0}，如果要应用并删除其他stash，命令：git stash pop stash@{$num} ，比如应用并删除第二个：git stash pop stash@{1}
（7）**git stash drop** stash@{$num} ：丢弃stash@{$num}存储，从列表中删除这个存储
（8）`**git stash clear** ：`删除所有缓存的stash
```
